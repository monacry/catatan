```
$invoice    = Divisi::where('tahun', $tahun)
            ->where('user_id', $user->id)
            // Pendapatan Debit
            ->withSum([
                'jurnals as pendapatan_debet' => function (Builder $q) {
                    $q->whereHas('daftarakun_debet', function ($q) {
                        $q->where('tipe_akun', 'Pendapatan');
                    });
                },
                'jurnals as pendapatan_kredit' => function (Builder $q) {
                    $q->whereHas('daftarakun_kredit', function ($q) {
                        $q->where('tipe_akun', 'Pendapatan');
                    });
                },
                'jurnals as hpp_debet' => function (Builder $q) {
                    $q->whereHas('daftarakun_debet', function ($q) {
                        $q->where('tipe_akun', 'Harga Pokok Penjualan');
                    });
                },
                'jurnals as hpp_kredit' => function (Builder $q) {
                    $q->whereHas('daftarakun_kredit', function ($q) {
                        $q->where('tipe_akun', 'Harga Pokok Penjualan');
                    });
                },
                'jurnals as biaya_debet' => function (Builder $q) {
                    $q->whereHas('daftarakun_debet', function ($q) {
                        $q->where('tipe_akun', 'Beban');
                    });
                },
                'jurnals as biaya_kredit' => function (Builder $q) {
                    $q->whereHas('daftarakun_kredit', function ($q) {
                        $q->where('tipe_akun', 'Beban');
                    });
                },
                'jurnals as pendapatan_lain_debet' => function (Builder $q) {
                    $q->whereHas('daftarakun_debet', function ($q) {
                        $q->where('tipe_akun', 'Pendapatan Lainnya');
                    });
                },
                'jurnals as pendapatan_lain_kredit' => function (Builder $q) {
                    $q->whereHas('daftarakun_kredit', function ($q) {
                        $q->where('tipe_akun', 'Pendapatan Lainnya');
                    });
                },
                'jurnals as biaya_lain_debet' => function (Builder $q) {
                    $q->whereHas('daftarakun_debet', function ($q) {
                        $q->where('tipe_akun', 'Biaya Lainnya');
                    });
                },
                'jurnals as biaya_lain_kredit' => function (Builder $q) {
                    $q->whereHas('daftarakun_kredit', function ($q) {
                        $q->where('tipe_akun', 'Biaya Lainnya');
                    });
                }
            ], 'saldo');

return DataTables::eloquent($invoice)
  ->editColumn('uuid', function ($data) {
      return $data->uuid;
  })
  ->editColumn('keterangan', function ($data) {
      return $data->keterangan;
  })
  ->editColumn('kode', function ($data) {
      return $data->kode;
  })
  ->editColumn('saldo_awal', function ($data) {
      return cetak($data->saldo_awal);
  })
  ->addColumn('pemasukan', function ($data) {
      $totalPendapatan        = $data->pendapatan_kredit - $data->pendapatan_debet;
      $totalPendapatanLain    = $data->pendapatan_lain_kredit - $data->pendapatan_lain_debet;
      $result                 = $totalPendapatan + $totalPendapatanLain;
      return cetak($result);
  })
  ->addColumn('pengeluaran', function ($data) {
      $totalHpp               = $data->hpp_debet - $data->hpp_kredit;
      $totalBiaya             = $data->biaya_debet - $data->biaya_kredit;
      $totalBiayaLainnya      = $data->biaya_lain_debet - $data->biaya_lain_kredit;
      $result                 = $totalHpp + $totalBiaya + $totalBiayaLainnya;
      return cetak($result);
  })
  ->addColumn('saldo_akhir', function ($data) {
      $totalPendapatan        = $data->pendapatan_kredit - $data->pendapatan_debet;
      $totalPendapatanLain    = $data->pendapatan_lain_kredit - $data->pendapatan_lain_debet;
  
      $totalHpp               = $data->hpp_debet - $data->hpp_kredit;
      $totalBiaya             = $data->biaya_debet - $data->biaya_kredit;
      $totalBiayaLainnya      = $data->biaya_lain_debet - $data->biaya_lain_kredit;
  
      $labaKotor              = $totalPendapatan - $totalHpp;
      $labaBersih             = $labaKotor - $totalBiaya;
      $jumlah                 = $labaBersih + $totalPendapatanLain - $totalBiayaLainnya;
      $saldoAkhir             = $data->saldo_awal + $jumlah;
      return cetak($saldoAkhir);
  })
  ->toJson();
```
