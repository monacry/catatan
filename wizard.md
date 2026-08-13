#laravel setup wizard view

```php
@extends('core._layouts.main')

@push('pageCSS')
    <link rel="stylesheet" href="{{ asset('assets/vendor/libs/bs-stepper/bs-stepper.css') }}" />
    <link rel="stylesheet" href="{{ asset('assets/vendor/libs/@form-validation/form-validation.css') }}" />
@endpush

@section('content')
    <div class="row justify-content-center">
        <div class="col-12 col-md-6 col-lg-7">
            <div class="card">
                <div class="card-header">
                    <div class="d-flex align-items-center">
                        <div class="avatar flex-shrink-0 me-2 avatar-md">
                            <span class="avatar-initial rounded bg-label-primary">
                                <i class='fs-3 ti tabler-folder-open-filled'></i>
                            </span>
                        </div>
                        <div class="d-flex w-100 flex-wrap justify-content-between align-items-center gap-1">
                            <div class="me-2">
                                <h5 class="mb-0">TAMBAH PEGAWAI</h5>
                                <p class="mb-0"></p>
                            </div>
                            <div class="row align-items-center">
                                <div class="d-flex gap-3">
                                    <a href="{{ route('pegawai.index') }}" class="btn btn-label-secondary waves-effect waves-light">
                                        <span class="icon-base ti tabler-arrow-back me-1"></span>Kembali
                                    </a>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="card-body">
                    <x-alert />
                    <div class="bs-stepper wizard-modern wizard-modern-example">
                        <div class="bs-stepper-header">
                            <div class="step" data-target="#data-utama">
                                <button type="button" class="step-trigger">
                                    <span class="bs-stepper-circle">1</span>
                                </button>
                            </div>
                            <div class="line">
                                <i class="icon-base ti tabler-chevron-right"></i>
                            </div>
                            <div class="step" data-target="#data-personal">
                                <button type="button" class="step-trigger">
                                    <span class="bs-stepper-circle">2</span>
                                </button>
                            </div>
                            <div class="line">
                                <i class="icon-base ti tabler-chevron-right"></i>
                            </div>
                            <div class="step" data-target="#data-kepegawaian">
                                <button type="button" class="step-trigger">
                                    <span class="bs-stepper-circle">3</span>
                                </button>
                            </div>
                            <div class="line">
                                <i class="icon-base ti tabler-chevron-right"></i>
                            </div>
                            <div class="step" data-target="#data-keuangan">
                                <button type="button" class="step-trigger">
                                    <span class="bs-stepper-circle">4</span>
                                </button>
                            </div>
                        </div>
                        <div class="bs-stepper-content">
                            <form id="wizard-validation-form" action="{{ route('pegawai.store') }}" method="POST">
                                @csrf
                                @method('POST')

                                {{-- DATA UTAMA --}}
                                <div id="data-utama" class="content">
                                    <div class="content-header mb-4">
                                        <h6 class="mb-0">Informasi Utama</h6>
                                        <small>isi kebutuhan untuk data utama.</small>
                                    </div>
                                    <div class="row mb-6">
                                        <label class="col-sm-2 col-form-label" for="basic-default-name">Name</label>
                                        <div class="col-sm-10">
                                            <input type="text" class="form-control" id="basic-default-name" placeholder="John Doe">
                                        </div>
                                    </div>
                                    <div class="row g-6">
                                        <div class="col-md-6 group">
                                            <label class="form-label" for="inputStep1NamaLengkap">
                                                <span class="text-danger">*</span>
                                                Nama Lengkap
                                            </label>
                                            <input type="text" name="step1_nama_lengkap" id="inputStep1NamaLengkap" value="{{ old('step1_nama_lengkap') }}" class="form-control form-control-sm" placeholder="Nama Lengkap" />
                                            <small class="invalid text-danger">
                                                @error('step1_nama_lengkap', 'pegawaiStore')
                                                    {{ $message }}
                                                @enderror
                                            </small>
                                        </div>

                                        <div class="col-md-6 group">
                                            <label class="form-label" for="inputStep1NomorTelpon">
                                                Nomor Telepon
                                            </label>
                                            <input type="text" name="step1_nomor_telepon" id="inputStep1NomorTelpon" value="{{ old('step1_nomor_telepon') }}" class="form-control form-control-sm" placeholder="Nama Lengkap" />
                                            <small class="invalid text-danger">
                                                @error('step1_nomor_telepon', 'pegawaiStore')
                                                    {{ $message }}
                                                @enderror
                                            </small>
                                        </div>

                                        <div class="col-md-6 group">
                                            <label class="form-label" for="inputStep1NomorIndukPegawai">
                                                <span class="text-danger">*</span>
                                                Nomor Induk Pegawai
                                            </label>
                                            <input type="text" name="step1_nomor_induk_pegawai" id="inputStep1NomorIndukPegawai" value="{{ old('step1_nomor_induk_pegawai') }}" class="form-control form-control-sm" placeholder="Nomor Induk Pegawai" />
                                            <small class="invalid text-danger">
                                                @error('step1_nomor_induk_pegawai', 'pegawaiStore')
                                                    {{ $message }}
                                                @enderror
                                            </small>
                                        </div>

                                        <div class="col-md-6 group">
                                            <label class="form-label" for="inputStep1IDPerangkatAbsensi">
                                                ID Perangkat Absensi
                                            </label>
                                            <input type="text" name="step1_id_perangkat_absensi" id="inputStep1IDPerangkatAbsensi" value="{{ old('step1_id_perangkat_absensi') }}" class="form-control form-control-sm" placeholder="ID Perangkat Absensi" />
                                            <small class="invalid text-danger">
                                                @error('step1_id_perangkat_absensi', 'pegawaiStore')
                                                    {{ $message }}
                                                @enderror
                                            </small>
                                        </div>


                                        <div class="col-12 d-flex justify-content-end">
                                            <button type="button" class="btn btn-primary btn-next"><span class="align-middle d-sm-inline-block d-none me-sm-1 me-0">Selanjutnya</span> <i class="icon-base ti tabler-arrow-right"></i></button>
                                        </div>
                                    </div>
                                </div>

                                {{-- DATA PERSONAL --}}
                                <div id="data-personal" class="content">
                                    <div class="content-header mb-4">
                                        <h6 class="mb-0">Informasi Pribadi</h6>
                                        <small>Fill all employee personal basic information data.</small>
                                    </div>
                                    <div class="row g-6">
                                        <div class="col-md-12 group">
                                            <label class="form-label" for="inputStep1NamaLengkap">
                                                <span class="text-danger">*</span>
                                                Nama Lengkap
                                            </label>
                                            <input type="text" name="step1_nama_lengkap" id="inputStep1NamaLengkap" value="{{ old('step1_nama_lengkap') }}" class="form-control" placeholder="Nama Lengkap" />
                                            <small class="invalid text-danger">
                                                @error('step1_nama_lengkap', 'pegawaiStore')
                                                    {{ $message }}
                                                @enderror
                                            </small>
                                        </div>

                                        <div class="col-md-6 group">
                                            <label class="form-label text-dark" for="inputStep1TempatLahir">
                                                <span class="text-danger">*</span>
                                                Tempat Lahir
                                            </label>
                                            <input type="text" name="step1_tempat_lahir" id="inputStep1TempatLahir" value="{{ old('step1_tempat_lahir') }}" class="form-control" placeholder="Tempat Lahir">
                                            <small class="invalid text-danger">
                                                @error('step1_tempat_lahir', 'pegawaiStore')
                                                    {{ $message }}
                                                @enderror
                                            </small>
                                        </div>

                                        <div class="col-md-6 group">
                                            <label class="form-label text-dark" for="inputStep1TanggalLahir">
                                                <span class="text-danger">*</span>
                                                Tanggal Lahir
                                            </label>
                                            <input type="text" name="step1_tanggal_lahir" id="inputStep1TanggalLahir" value="{{ old('step1_tanggal_lahir') }}" class="form-control __datepicker" placeholder="Tanggal Lahir">
                                            <small class="invalid text-danger">
                                                @error('step1_tanggal_lahir', 'pegawaiStore')
                                                    {{ $message }}
                                                @enderror
                                            </small>
                                        </div>

                                        <div class="col-md-6 group">
                                            <label class="form-label" for="inputStep1JenisKelamin">
                                                <span class="text-danger">*</span>
                                                Jenis Kelamin
                                            </label>
                                            <select name="step1_jenis_kelamin" id="inputStep1JenisKelamin" class="form-select __selectpicker w-100" data-style="btn-default" data-live-search="true">
                                                @foreach ($pilihanJenisKelamin as $item)
                                                    <option value="{{ $item->id }}" @selected(old('step1_jenis_kelamin') == $item->id)>{{ $item->keterangan }}</option>
                                                @endforeach
                                            </select>
                                            <small class="invalid text-danger">
                                                @error('step1_jenis_kelamin', 'pegawaiStore')
                                                    {{ $message }}
                                                @enderror
                                            </small>
                                        </div>

                                        <div class="col-md-6 group">
                                            <label class="form-label" for="inputStep1StatusPernikahan">
                                                Status Pernikahan
                                            </label>
                                            <select name="step1_status_pernikahan" id="inputStep1StatusPernikahan" class="form-select __selectpicker w-100" data-style="btn-default" data-live-search="true">
                                                <option value="">Tidak Di Atur</option>
                                                @foreach ($pilihanStatusPernikahan as $item)
                                                    <option value="{{ $item->id }}" @selected(old('step1_status_pernikahan') == $item->id)>{{ $item->keterangan }}</option>
                                                @endforeach
                                            </select>
                                            <small class="invalid text-danger">
                                                @error('step1_status_pernikahan', 'pegawaiStore')
                                                    {{ $message }}
                                                @enderror
                                            </small>
                                        </div>

                                        <div class="col-md-6 group">
                                            <label class="form-label" for="inputStep1Agama">
                                                Agama
                                            </label>
                                            <select name="step1_agama" id="inputStep1Agama" class="form-select __selectpicker w-100" data-style="btn-default" data-live-search="true">
                                                <option value="">Tidak Di Atur</option>
                                                @foreach ($pilihanAgama as $item)
                                                    <option value="{{ $item->id }}" @selected(old('step1_agama') == $item->id)>{{ $item->keterangan }}</option>
                                                @endforeach
                                            </select>
                                            <small class="invalid text-danger">
                                                @error('step1_agama', 'pegawaiStore')
                                                    {{ $message }}
                                                @enderror
                                            </small>
                                        </div>

                                        <div class="col-md-6 group">
                                            <label class="form-label" for="inputStep1GolonganDarah">
                                                Golongan Darah
                                            </label>
                                            <select name="step1_golongan_darah" id="inputStep1GolonganDarah" class="form-select __selectpicker w-100" data-style="btn-default" data-live-search="true">
                                                <option value="">Tidak Di Atur</option>
                                                @foreach ($pilihanGolonganDarah as $item)
                                                    <option value="{{ $item->id }}" @selected(old('step1_golongan_darah') == $item->id)>{{ $item->keterangan }}</option>
                                                @endforeach
                                            </select>
                                            <small class="invalid text-danger">
                                                @error('step1_golongan_darah', 'pegawaiStore')
                                                    {{ $message }}
                                                @enderror
                                            </small>
                                        </div>



                                        <div class="col-12 d-flex justify-content-end">
                                            <button type="button" class="btn btn-primary btn-next"><span class="align-middle d-sm-inline-block d-none me-sm-1 me-0">Selanjutnya</span> <i class="icon-base ti tabler-arrow-right"></i></button>
                                        </div>
                                    </div>
                                </div>

                                {{-- DATA KEPEGAWAIAN --}}
                                <div id="data-kepegawaian" class="content">
                                    <div class="content-header mb-4">
                                        <h6 class="mb-0">Data Kepegawaian</h6>
                                        <small>Atur data kepegawaian terkait perusahaan</small>
                                    </div>
                                    <div class="row g-6">
                                        {{-- kode pegawai --}}
                                        <div class="col-md-12 group">
                                            <label class="form-label" for="inputStep2KodePegawai">
                                                <span class="text-danger">*</span>
                                                Kode Pegawai
                                            </label>
                                            <input type="text" name="step2_kode_pegawai" id="inputStep2KodePegawai" value="{{ old('step2_kode_pegawai') }}" class="form-control" placeholder="Kode Pegawai" />
                                            <small class="invalid text-danger">
                                                @error('step2_kode_pegawai', 'pegawaiStore')
                                                    {{ $message }}
                                                @enderror
                                            </small>
                                        </div>

                                        {{-- departemen --}}
                                        <div class="col-md-6 group">
                                            <label class="form-label" for="inputStep2Departemen">
                                                <span class="text-danger">*</span>
                                                Departemen
                                            </label>
                                            <select name="step2_departemen" id="inputStep2Departemen" class="form-select __selectpicker w-100" data-style="btn-default" data-live-search="true">
                                                @if ($pilihanDepartemen->isNotEmpty())
                                                    @foreach ($pilihanDepartemen as $item)
                                                        <option value="{{ $item->uuid }}" @selected(old('step2_departemen') == $item->uuid)>{{ $item->nama }}</option>
                                                    @endforeach
                                                @else
                                                    <option>--belum ada status pegawai--</option>
                                                @endif
                                            </select>
                                            <small class="invalid text-danger">
                                                @error('step2_departemen', 'pegawaiStore')
                                                    {{ $message }}
                                                @enderror
                                            </small>
                                        </div>

                                        {{-- status pegawai --}}
                                        <div class="col-md-6 group">
                                            <label class="form-label" for="inputStep2StatusPegawai">
                                                <span class="text-danger">*</span>
                                                Status Pegawai
                                            </label>
                                            <select name="step2_status_pegawai" id="inputStep2StatusPegawai" class="form-select __selectpicker w-100" data-style="btn-default" data-live-search="true">
                                                @if ($pilihanPegawaiStatus->isNotEmpty())
                                                    @foreach ($pilihanPegawaiStatus as $item)
                                                        <option value="{{ $item->uuid }}" @selected(old('step2_status_pegawai') == $item->uuid) data-status="{{ $item->tipe }}">{{ $item->nama }}</option>
                                                    @endforeach
                                                @else
                                                    <option>--belum ada status pegawai--</option>
                                                @endif
                                            </select>
                                            <small class="invalid text-danger">
                                                @error('step2_status_pegawai', 'pegawaiStore')
                                                    {{ $message }}
                                                @enderror
                                            </small>
                                        </div>

                                        {{-- awal kontrak --}}
                                        <div class="col-md-6 group">
                                            <label class="form-label text-dark" for="inputStep2AwalKontrak">
                                                <span class="text-danger">*</span>
                                                Awal Kontrak
                                            </label>
                                            <input type="text" name="step2_awal_kontrak" id="inputStep2AwalKontrak" value="{{ old('step2_awal_kontrak') }}" class="form-control __datepicker" placeholder="Awal Kontrak">
                                            <small class="invalid text-danger">
                                                @error('step2_awal_kontrak', 'pegawaiStore')
                                                    {{ $message }}
                                                @enderror
                                            </small>
                                        </div>

                                        {{-- akhir kontrak --}}
                                        <div class="col-md-6 group">
                                            <label class="form-label text-dark" for="inputStep2AkhirKontrak">
                                                Akhir Kontrak
                                            </label>
                                            <input type="text" name="step2_akhir_kontrak" id="inputStep2AkhirKontrak" value="{{ old('step2_akhir_kontrak') }}" class="form-control __datepicker" placeholder="Akhir Kontrak">
                                            <small class="invalid text-danger">
                                                @error('step2_akhir_kontrak', 'pegawaiStore')
                                                    {{ $message }}
                                                @enderror
                                            </small>
                                        </div>

                                        {{-- status ptkp --}}
                                        <div class="col-md-6 group">
                                            <label class="form-label" for="inputStep2StatusPTKP">
                                                Status PTKP
                                            </label>
                                            <select name="step2_status_ptkp" id="inputStep2StatusPTKP" class="form-select __selectpicker w-100" data-style="btn-default" data-live-search="true">
                                                @if ($pilihanPTKP->isNotEmpty())
                                                    @foreach ($pilihanPTKP as $item)
                                                        <option value="{{ $item->id }}" @selected(old('step2_status_ptkp') == $item->id)>{{ $item->nama }}</option>
                                                    @endforeach
                                                @else
                                                    <option>--belum ada status ptkp--</option>
                                                @endif
                                            </select>
                                            <small class="invalid text-danger">
                                                @error('step2_status_ptkp', 'pegawaiStore')
                                                    {{ $message }}
                                                @enderror
                                            </small>
                                        </div>

                                        {{-- Nomor NPWP --}}
                                        <div class="col-md-6 group">
                                            <label class="form-label" for="inputStep2NomorNPWP">
                                                Nomor NPWP
                                            </label>
                                            <input type="text" name="step2_nomor_npwp" id="inputStep2NomorNPWP" value="{{ old('step2_nomor_npwp') }}" class="form-control" placeholder="Nomor NPWP" />
                                            <small class="invalid text-danger">
                                                @error('step2_nomor_npwp', 'pegawaiStore')
                                                    {{ $message }}
                                                @enderror
                                            </small>
                                        </div>

                                        <div class="col-12 d-flex justify-content-between">
                                            <button type="button" class="btn btn-label-primary btn-prev">
                                                <i class="icon-base ti tabler-arrow-left me-sm-1 me-0"></i>
                                                <span class="align-middle d-sm-inline-block d-none">Sebelumnya</span>
                                            </button>
                                            <button type="button" class="btn btn-primary btn-next">
                                                <span class="align-middle d-sm-inline-block d-none me-sm-1 me-0">Selanjutnya</span>
                                                <i class="icon-base ti tabler-arrow-right"></i>
                                            </button>
                                        </div>
                                    </div>
                                </div>

                                {{-- DATA KEUANGAN --}}
                                <div id="data-keuangan" class="content">
                                    <div class="content-header mb-4">
                                        <h6 class="mb-0">Data Keuangan</h6>
                                        <small>data terkait keuangan dan penggajian.</small>
                                    </div>
                                    <div class="row g-6">
                                        {{-- Nama Bank --}}
                                        <div class="col-md-6 group">
                                            <label class="form-label" for="inputStep3NamaBank">
                                                Nama Bank
                                            </label>
                                            <input type="text" name="step3_nama_bank" id="inputStep3NamaBank" value="{{ old('step3_nama_bank') }}" class="form-control" placeholder="Nama Bank" />
                                            <small class="invalid text-danger">
                                                @error('step3_nama_bank', 'pegawaiStore')
                                                    {{ $message }}
                                                @enderror
                                            </small>
                                        </div>

                                        {{-- Atas Nama --}}
                                        <div class="col-md-6 group">
                                            <label class="form-label" for="inputStep3AtasNamaBank">
                                                Atas Nama
                                            </label>
                                            <input type="text" name="step3_atas_nama_bank" id="inputStep3AtasNamaBank" value="{{ old('step3_atas_nama_bank') }}" class="form-control" placeholder="Atas Nama" />
                                            <small class="invalid text-danger">
                                                @error('step3_atas_nama_bank', 'pegawaiStore')
                                                    {{ $message }}
                                                @enderror
                                            </small>
                                        </div>

                                        {{-- Nomor Rekening --}}
                                        <div class="col-md-6 group">
                                            <label class="form-label" for="inputStep3NomorRekeningBank">
                                                Nomor Rekening
                                            </label>
                                            <input type="text" name="step3_nomor_rekening_bank" id="inputStep3NomorRekeningBank" value="{{ old('step3_nomor_rekening_bank') }}" class="form-control" placeholder="Nomor Rekening" />
                                            <small class="invalid text-danger">
                                                @error('step3_nomor_rekening_bank', 'pegawaiStore')
                                                    {{ $message }}
                                                @enderror
                                            </small>
                                        </div>

                                        {{-- Gaji Pokok --}}
                                        <div class="col-md-6 group">
                                            <label class="form-label" for="inputStep3GajiPokok">
                                                Gaji Pokok
                                            </label>
                                            <input type="text" name="step3_gaji_pokok" id="inputStep3GajiPokok" value="{{ old('step3_gaji_pokok') }}" class="form-control __inputmask" placeholder="Gaji Pokok" />
                                            <small class="invalid text-danger">
                                                @error('step3_gaji_pokok', 'pegawaiStore')
                                                    {{ $message }}
                                                @enderror
                                            </small>
                                        </div>
                                        <hr />
                                        <div class="col-md-6">
                                            <div class="row g-6">
                                                <div class="col-12 group">
                                                    <label class="form-label" for="inputStep3nomorBPJSKesehatan">
                                                        Nomor BPJS Kesehatan
                                                    </label>
                                                    <input type="text" name="step3_nomor_bpjs_kesehatan" id="inputStep3nomorBPJSKesehatan" value="{{ old('step3_nomor_bpjs_kesehatan') }}" class="form-control" placeholder="Nomor BPJS Kesehatan">
                                                    <small class="invalid text-danger">
                                                        @error('step3_nomor_bpjs_kesehatan', 'pegawaiStore')
                                                            {{ $message }}
                                                        @enderror
                                                    </small>
                                                </div>
                                                <div class="col-12 group">
                                                    <label class="form-label" for="inputStep3KbuBPJSKesehatan">
                                                        KBU
                                                    </label>
                                                    <select name="step3_kbu_bpjs_kesehatan" id="inputStep3KbuBPJSKesehatan" class="form-select __selectpicker w-100" data-style="btn-default" data-live-search="true">
                                                        @if ($bpjsKS->isNotEmpty())
                                                            <option hidden></option>
                                                            @foreach ($bpjsKS as $item)
                                                                <option value="{{ $item->uuid }}" @selected(old('step3_kbu_bpjs_kesehatan') == $item->uuid)>{{ $item->nomor_kbu }} ({{ $item->keterangan }})</option>
                                                            @endforeach
                                                        @endif
                                                    </select>
                                                    <small class="invalid text-danger">
                                                        @error('step3_kbu_bpjs_kesehatan', 'pegawaiStore')
                                                            {{ $message }}
                                                        @enderror
                                                    </small>
                                                </div>
                                            </div>
                                        </div>
                                        <div class="col-md-6">
                                            <div class="row g-6">
                                                <div class="col-12 group">
                                                    <label class="form-label" for="inputStep3NomorBPJSKetenagakerjaan">
                                                        Nomor BPJS Ketenagakerjaan
                                                    </label>
                                                    <input type="text" name="step3_nomor_bpjs_ketenagakerjaan" id="inputStep3NomorBPJSKetenagakerjaan" value="{{ old('step3_nomor_bpjs_ketenagakerjaan') }}" class="form-control" placeholder="Nomor BPJS Ketenagakerjaan">
                                                    <small class="invalid text-danger">
                                                        @error('step3_nomor_bpjs_ketenagakerjaan', 'pegawaiStore')
                                                            {{ $message }}
                                                        @enderror
                                                    </small>
                                                </div>
                                                <div class="col-12 group">
                                                    <label class="form-label" for="inputStep3NppBPJSKetenagakerjaan">
                                                        NPP
                                                    </label>
                                                    <select name="step3_npp_bpjs_ketenagakerjaan" id="inputStep3NppBPJSKetenagakerjaan" class="form-select __selectpicker w-100" data-style="btn-default" data-live-search="true">
                                                        @if ($bpjsKT->isNotEmpty())
                                                            <option hidden></option>
                                                            @foreach ($bpjsKT as $item)
                                                                <option value="{{ $item->uuid }}" @selected(old('step3_npp_bpjs_ketenagakerjaan') == $item->uuid)>{{ $item->nomor_npp }} ({{ $item->nama_npp }})</option>
                                                            @endforeach
                                                        @endif
                                                    </select>
                                                    <small class="invalid text-danger">
                                                        @error('step3_npp_bpjs_ketenagakerjaan', 'pegawaiStore')
                                                            {{ $message }}
                                                        @enderror
                                                    </small>
                                                </div>
                                            </div>
                                        </div>

                                        <div class="col-12 d-flex justify-content-between">
                                            <button type="button" class="btn btn-label-primary btn-prev">
                                                <i class="icon-base ti tabler-arrow-left me-sm-1 me-0"></i>
                                                <span class="align-middle d-sm-inline-block d-none">Sebelumnya</span>
                                            </button>
                                            <button type="submit" class="btn btn-primary btn-submit">
                                                <i class="icon-base ti tabler-send-filled"></i>
                                                <span class="align-middle d-sm-inline-block d-none ms-sm-1 me-0">Kirim</span>
                                            </button>
                                        </div>
                                    </div>
                                </div>
                            </form>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
@endsection

{{-- TAMBAH PEGAWAI --}}
@push('pageJS')
    <script src="{{ asset('assets/vendor/libs/bs-stepper/bs-stepper.js') }}"></script>
    <script src="{{ asset('assets/vendor/libs/@form-validation/popular.js') }}"></script>
    <script src="{{ asset('assets/vendor/libs/@form-validation/bootstrap5.js') }}"></script>
    <script>
        // triger akhir kontrak disable dan enable
        $(function() {
            // Fungsi untuk handle enable/disable akhir kontrak
            function toggleAkhirKontrak() {
                var selected = $('#inputStep2StatusPegawai').find(':selected');
                var tipe = selected.data('status');
                if (tipe === 'pkwtt') {
                    $('#inputStep2AkhirKontrak').prop('disabled', true).val('');
                } else {
                    $('#inputStep2AkhirKontrak').prop('disabled', false);
                }
            }

            // Trigger saat select berubah
            $('#inputStep2StatusPegawai').on('change', function() {
                toggleAkhirKontrak();
            });

            // Inisialisasi saat halaman dimuat
            toggleAkhirKontrak();
        });

        $(function() {
            const buttonNext = $('.btn-next')
            const buttonPrev = $('.btn-prev')
            const buttonSubmit = $('.btn-submit')
            const stepperEl = document.querySelector('.wizard-modern-example')
            if (typeof stepperEl !== undefined && stepperEl !== null) {
                const stepper = new Stepper(stepperEl, {
                    linear: true,
                    animation: true
                });

                // FORM VALIDASI
                const validationForm = $('#wizard-validation-form')

                // validasi step1
                const validationStep1 = FormValidation.formValidation(validationForm[0], {
                    fields: {
                        step1_nama_lengkap: {
                            validators: {
                                notEmpty: {
                                    message: 'Input tidak boleh kosong'
                                },
                                stringLength: {
                                    max: 100,
                                    message: 'Input maksimal 100 karakter'

                                }
                            }
                        },
                    },
                    plugins: {
                        trigger: new FormValidation.plugins.Trigger(),
                    }
                });

                // validasi step2
                const validationStep2 = FormValidation.formValidation(validationForm[0], {
                    fields: {
                        step1_nama_lengkap: {
                            validators: {
                                notEmpty: {
                                    message: 'Input tidak boleh kosong'
                                },
                                stringLength: {
                                    max: 100,
                                    message: 'Input maksimal 100 karakter'

                                }
                            }
                        },
                        step1_tempat_lahir: {
                            validators: {
                                notEmpty: {
                                    message: 'Input tidak boleh kosong'
                                },
                                stringLength: {
                                    max: 100,
                                    message: 'Input maksimal 100 karakter'
                                }
                            }
                        },
                        step1_tanggal_lahir: {
                            validators: {
                                notEmpty: {
                                    message: 'Input tidak boleh kosong'
                                },
                                date: {
                                    format: 'DD-MM-YYYY',
                                    message: 'Format tanggal tidak sesuai.(ex: 01-12-2005)'
                                }
                            }
                        },
                        step1_jenis_kelamin: {
                            validators: {
                                notEmpty: {
                                    message: 'Input tidak boleh kosong'
                                },
                                callback: {
                                    message: 'Nilai input tidak sesuai',
                                    callback: function(input) {
                                        return ['1', '2'].includes(input.value);
                                    }
                                }
                            }
                        },
                        step1_status_pernikahan: {
                            validators: {
                                callback: {
                                    message: 'Nilai input tidak sesuai',
                                    callback: function(input) {
                                        if (input.value === '') {
                                            return true;
                                        }
                                        return ['1', '2', '3', '4'].includes(input.value);
                                    }
                                }
                            }
                        },
                        step1_agama: {
                            validators: {
                                callback: {
                                    message: 'Nilai input tidak sesuai',
                                    callback: function(input) {
                                        if (input.value === '') {
                                            return true;
                                        }
                                        return ['1', '2', '3', '4', '5', '6'].includes(input.value);
                                    }
                                }
                            }
                        },
                        step1_golongan_darah: {
                            validators: {
                                callback: {
                                    message: 'Nilai input tidak sesuai',
                                    callback: function(input) {
                                        if (input.value === '') {
                                            return true;
                                        }
                                        return ['1', '2', '3', '4'].includes(input.value);
                                    }
                                }
                            }
                        },
                    },
                    plugins: {
                        trigger: new FormValidation.plugins.Trigger(),
                    }
                });

                // validasi step3
                const validationStep3 = FormValidation.formValidation(validationForm[0], {
                    fields: {
                        step2_kode_pegawai: {
                            validators: {
                                notEmpty: {
                                    message: 'Input tidak boleh kosong'
                                },
                                stringLength: {
                                    max: 100,
                                    message: 'Input maksimal 100 karakter'

                                }
                            }
                        },
                        step2_departemen: {
                            validators: {
                                notEmpty: {
                                    message: 'Input tidak boleh kosong'
                                }
                            }
                        },
                        step2_status_pegawai: {
                            validators: {
                                notEmpty: {
                                    message: 'Input tidak boleh kosong'
                                }
                            }
                        },
                        step2_awal_kontrak: {
                            validators: {
                                notEmpty: {
                                    message: 'Input tidak boleh kosong'
                                },
                                date: {
                                    format: 'DD-MM-YYYY',
                                    message: 'Format tanggal tidak sesuai.(ex: 01-12-2005)'
                                }
                            }
                        },
                        step2_akhir_kontrak: {
                            validators: {
                                date: {
                                    format: 'DD-MM-YYYY',
                                    message: 'Format tanggal tidak sesuai.(ex: 01-12-2005)'
                                }
                            }
                        },
                        step2_status_ptkp: {
                            validators: {
                                integer: {
                                    message: 'Input tidak sesuai'
                                }
                            }
                        },
                    },
                    plugins: {
                        trigger: new FormValidation.plugins.Trigger(),
                    }
                });

                // validasi step4
                const validationStep4 = FormValidation.formValidation(validationForm[0], {
                    fields: {
                        step3_nama_bank: {
                            validators: {
                                stringLength: {
                                    max: 50,
                                    message: 'Input maksimal 50 karakter'

                                }
                            }
                        },
                        step3_atas_nama_bank: {
                            validators: {
                                stringLength: {
                                    max: 50,
                                    message: 'Input maksimal 50 karakter'

                                }
                            }
                        },
                        step3_nomor_rekening_bank: {
                            validators: {
                                stringLength: {
                                    max: 50,
                                    message: 'Input maksimal 50 karakter'
                                }
                            }
                        },
                        step3_gaji_pokok: {
                            validators: {
                                stringLength: {
                                    max: 21,
                                    message: 'Input terlalu panjang'
                                }
                            }
                        },
                        step3_nomor_bpjs_kesehatan: {
                            validators: {
                                stringLength: {
                                    max: 30,
                                    message: 'Input maksimal 30 karakter'
                                }
                            }
                        },
                        step3_kbu_bpjs_kesehatan: {
                            validators: {
                                stringLength: {
                                    max: 30,
                                    message: 'Input maksimal 30 karakter'

                                }
                            }
                        },
                        step3_nomor_bpjs_ketenagakerjaan: {
                            validators: {
                                stringLength: {
                                    max: 30,
                                    message: 'Input maksimal 30 karakter'

                                }
                            }
                        },
                        step3_npp_bpjs_ketenagakerjaan: {
                            validators: {
                                stringLength: {
                                    max: 30,
                                    message: 'Input maksimal 30 karakter'

                                }
                            }
                        },
                    },
                    plugins: {
                        trigger: new FormValidation.plugins.Trigger(),
                    }
                });

                eventValidation(validationStep1)
                eventValidation(validationStep2)
                eventValidation(validationStep3)

                buttonNext.off('click').on('click', async function() {
                    const btn = $(this)
                    btn.prop('disabled', true).addClass('disabled')
                    let currentIndex = stepper._currentIndex + 1
                    switch (currentIndex) {
                        case 1:
                            try {
                                let status = await validationStep1.validate()
                                if (status === 'Valid') stepper.next()
                            } finally {
                                btn.prop('disabled', false).removeClass('disabled')
                            }
                            break;
                        case 2:
                            try {
                                let status = await validationStep2.validate()
                                if (status === 'Valid') stepper.next()
                            } finally {
                                btn.prop('disabled', false).removeClass('disabled')
                            }
                            break;
                        default:
                            stepper.to(1)
                            break;
                    }
                })

                buttonPrev.off('click').on('click', function() {
                    stepper.previous()
                })

                // step 3 atau saat data di submit
                validationForm.off('submit').on('submit', async function(e) {
                    e.preventDefault()
                    const form = this
                    buttonSubmit.prop('disabled', true).addClass('disabled');
                    try {
                        let status = await validationStep3.validate();
                        console.log(status);
                        if (status === 'Valid') {
                            form.submit()
                        }
                    } finally {
                        buttonSubmit.prop('disabled', false).removeClass('disabled');
                    }
                });

            }

            function eventValidation(validationStep) {
                // bersihkan semua nilai message validasi
                validationStep.on('core.element.validating', function(e) {
                    const group = $(e.element).closest('.group');
                    group.find('.invalid').empty();
                });

                // inisialisasi nilai message validasi
                validationStep.on('core.validator.validated', function(e) {
                    const group = $(e.element).closest('.group');
                    const invalidEl = group.find('.invalid');
                    if (!e.result.valid) {
                        invalidEl.html(e.result.message)
                    }
                });
            }
        })
    </script>
@endpush

```
