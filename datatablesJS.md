```javascript
<javascript>
  if (dt_piutang_table) {
            dt_piutang = new DataTable(dt_piutang_table, {
                scrollX: true,
                stateSave: true,
                serverSide: true,
                pageLength: 20,
                lengthMenu: [20, 50, 100],
                ajax: {
                    url: "{{ route('hutang-piutang.index-data-piutang') }}",
                    type: 'get',
                },
                columns: [{
                        data: 'uuid',
                        orderable: false,
                        searchable: false,
                        className: 'py-1 text-center text-nowrap',
                        width: '1%',
                    },
                    {
                        data: 'kode',
                        name: 'kode',
                        orderable: true,
                        searchable: true,
                        className: 'py-1 text-start',
                        // width: '5%'
                    },
                    {
                        data: 'keterangan',
                        name: 'keterangan',
                        orderable: true,
                        searchable: true,
                        className: 'py-1 text-start',
                        // width: '5%'
                    },
                    {
                        data: 'jenis',
                        name: 'jenis',
                        orderable: true,
                        searchable: true,
                        className: 'py-1 text-center text-nowrap',
                        // width: '5%'
                    },
                    {
                        data: 'saldo_awal',
                        name: 'saldo_awal',
                        orderable: true,
                        searchable: true,
                        className: 'py-1 text-end text-nowrap',
                        // width: '5%'
                    },
                    {
                        data: 'penambahan',
                        name: 'penambahan',
                        orderable: true,
                        searchable: true,
                        className: 'py-1 text-end text-nowrap',
                        // width: '5%'
                    },
                    {
                        data: 'pengurangan',
                        name: 'pengurangan',
                        orderable: true,
                        searchable: true,
                        className: 'py-1 text-end text-nowrap',
                        // width: '5%'
                    },
                    {
                        data: null,
                        name: 'saldo_akhir',
                        orderable: false,
                        searchable: false,
                        className: 'py-1 text-end text-nowrap',
                        render: function(data, type, full, meta) {
                            let saldoAwal = konversiNilaiMurni(full.saldo_awal) || 0;
                            let penambahan = konversiNilaiMurni(full.penambahan) || 0
                            let pengurangan = konversiNilaiMurni(full.pengurangan) || 0;
                            let saldoAkhir = saldoAwal + penambahan - pengurangan;
                            return new Intl.NumberFormat('id-ID', {
                                style: 'currency',
                                currency: 'IDR',
                                minimumFractionDigits: 0
                            }).format(saldoAkhir);
                        }
                    }
                ],
                columnDefs: [{
                    targets: 0,
                    render: function(data, type, full, meta) {
                        let urlEdit = "{{ route('daftar-nota.edit', ':dataUUID') }}".replace(':dataUUID', data)
                        let urlDestroy = "{{ route('daftar-nota.destroy', ':dataUUID') }}".replace(':dataUUID', data)
                        return (
                            `<div class="d-flex gap-2">` +
                            `<a href="${urlEdit}" class="btn btn-sm btn-primary py-0"><span class="icon-base bx bxs-edit-alt me-1"></span>Edit</a>` +
                            `<button data-uuid="${full.uuid}" data-kode="${full.kode}" class="btn btn-sm btn-danger py-0 __button-delete"><span class="icon-base bx bxs-trash me-1"></span>Delete</button>` +
                            `</div>`
                        )
                    }

                }],
                order: [
                    [1, 'asc']
                ],
            });

            function konversiNilaiMurni(nilai) {
                if (!nilai) return 0;
                let stringNilai = nilai.toString().trim();
                // JIKA nilainya hanya berisi satu karakter minus "-" atau string kosong, langsung kembalikan 0
                if (stringNilai === '-' || stringNilai === '') return 0;
                // Hapus semua karakter kecuali angka (0-9) dan minus (-)
                let angkaBersih = stringNilai.replace(/[^0-9-]/g, '');
                // Jika setelah dibersihkan hasilnya hanya "-" atau kosong, kembalikan 0
                if (angkaBersih === '-' || angkaBersih === '') return 0;
                return parseFloat(angkaBersih) || 0;
            }
        }
  }
</script>
```
