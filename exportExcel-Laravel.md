```php
use PhpOffice\PhpSpreadsheet\Spreadsheet;
use PhpOffice\PhpSpreadsheet\Writer\Xlsx;
use PhpOffice\PhpSpreadsheet\IOFactory;

public function templateExcel(){
  $tahun          = Cookie::get('tahun');
  $filePath       = 'template-excel/daftar-aset/daftaraset-' . $tahun . '.xlsx';
  if (Storage::disk('public')->exists($filePath)) {
    return response()->download(storage_path('app/public/' . $filePath));
  }
  $path           = storage_path('app/public/template-excel/daftar-aset/daftaraset-template.xlsx');
  $spreadsheet    = IOFactory::load($path);
  $sheet          = $spreadsheet->getActiveSheet();
  
  $tahunIni       = date('d-m-Y', strtotime('last day of December ' . $tahun));
  $tahunLalu      = date('d-m-Y', strtotime('last day of December ' . $tahun - 1));
  
  $sheet->setCellValue('E1', 'NILAI PEROLEHAN PER: ' . $tahunLalu);
  $sheet->setCellValue('F1', 'AKUMULASI PENYUSUTAN PER: ' . $tahunLalu);
  $sheet->setCellValue('G1', 'NILAI BUKU PER: ' . $tahunLalu);
  $sheet->setCellValue('I1', 'NILAI PEROLEHAN PER: ' . $tahunIni);
  $sheet->setCellValue('J1', 'DASAR PENYUSUTAN PER: ' . $tahunIni);
  $sheet->setCellValue('L1', 'BIAYA PENYUSUTAN PER: ' . $tahunIni);
  $sheet->setCellValue('M1', 'AKUMULASI PENYUSUTAN PER: ' . $tahunIni);
  $sheet->setCellValue('N1', 'NILAI BUKU PER: ' . $tahunIni);
  
  $sheet2         = $spreadsheet->getSheetByName('PENJELASAN');
  $tipeAkun       = Daftarakun::TahunUser($tahun)->where('tipe_akun', 'Aktiva Tetap')->where('akumulasi', false)->get();
  $startRow       = 4;
  foreach ($tipeAkun as $index => $akun) {
    $sheet2->setCellValue('A' . ($startRow + $index), $index + 1);
    $sheet2->setCellValue('B' . ($startRow + $index), $akun->kode);
    $sheet2->setCellValue('C' . ($startRow + $index), $akun->keterangan);
  }

  $newFileName    = 'daftaraset-' . $tahun . '.xlsx';
  $newFilePath    = storage_path('app/public/template-excel/daftar-aset/' . $newFileName);
  $writer         = new Xlsx($spreadsheet);
  $writer->save($newFilePath);
  return response()->download($newFilePath);
}
```
