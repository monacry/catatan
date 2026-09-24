```php
public function generate(Request $request)
    {
        /** @var User $user */
        $user           = Auth::user();
        $tahun          = Cookie::get('tahun'  .  auth()->user()->id);
        $input          = $request->input();
        $persediaan     = Persediaan::where('uuid', $input['transfer_gudang_persediaan_id'])->TahunUser($tahun)->first();
        if (!$persediaan) {
            return 'persediaan tidak dapat ditemukan';
        }

        $tanggalAkhir = Carbon::parse($input['transfer_gudang_tanggal'])->format('Y-m-d H:i:s');

        $gudang = JurnalPersediaan::with('jurnal')
            ->where('persediaan_id', $persediaan->id)
            ->where(function ($query) {
                $query->where('this_in', 1)->orWhere('this_out', 1);
            })
            ->where('tanggal', '<=', $tanggalAkhir)
            ->orderBy('tanggal', 'ASC')
            ->get()
            // ->unique(function ($item) {
            //     return $item->jurnal->keterangan_j . '|' . $item->jurnal->tanggal;
            // })
            ->groupBy(function ($item) {
                return $item->gudang_id ?? 'utama';
            });


        $dataGudang         = $gudang->get('utama', collect());
        $entriStok          = [];
        $debug              = [];
        $out                = (int) $input['transfer_gudang_quantity'];
        $pemakaianJumlah    = 0;

        // Awalan stok jika ada
        if ($persediaan->Q != 0 && $persediaan->harga_beli_akhir != 0) {
            $entriStok[] = [
                'in'        => $persediaan->Q,
                'harga'     => $persediaan->harga_beli_akhir,
                'jumlah'    => $persediaan->Q * $persediaan->harga_beli_akhir,
            ];
        }

        // return $entriStok;

        foreach ($dataGudang as $index => $item) {
            $penerimaanQ        = is_null($item['in']) || $item['in'] == 0 ? null : $item['in'];
            $pemakaianQ         = is_null($item['out']) || $item['out'] == 0 ? null : $item['out'];
            $hargaPenerimaan    = $penerimaanQ ? $item['saldo'] / $penerimaanQ : 0;

            if ($penerimaanQ > 0) {
                $entriStok[]    = [
                    'in'        => $penerimaanQ,
                    'harga'     => $hargaPenerimaan,
                    'jumlah'    => $penerimaanQ * $hargaPenerimaan,
                ];
            }
            // $debug[] = $entriStok;
            if ($pemakaianQ > 0 && !empty($entriStok)) {
                while ($pemakaianQ > 0 && !empty($entriStok)) {
                    $entriPertama = &$entriStok[0];
                    if ($entriPertama['in'] >= $pemakaianQ) { // Jika stok pertama lebih besar dari pemakaian sebelumnya
                        $entriPertama['in'] -= $pemakaianQ;
                        $entriPertama['jumlah'] -= $pemakaianQ * $entriPertama['harga'];
                        $pemakaianQ = 0;
                    } else { // Jika stok pertama lebih kecil dari pemakaian sebelumnya
                        $pemakaianQ -= $entriPertama['in'];
                        array_shift($entriStok);
                    }
                }
            }
        }

        if ($out > 0 && !empty($entriStok)) {
            while ($out > 0 && !empty($entriStok)) {
                $entriPertama = &$entriStok[0];
                if ($entriPertama['in'] >= $out) { // Jika stok pertama lebih besar dari qty out
                    $pemakaianJumlah += $out * $entriPertama['harga'];
                    $entriPertama['in'] -= $out;
                    $entriPertama['jumlah'] -= $out * $entriPertama['harga'];
                    $out = 0;
                } else { // Jika qty out lebih besar dari stok pertama
                    $pemakaianJumlah += $entriPertama['in'] * $entriPertama['harga'];
                    $out -= $entriPertama['in'];
                    array_shift($entriStok);
                }
            }
        }
        return $entriStok;
        return $debug;
        return $pemakaianJumlah;
    }
```
