```php
        return $query->map(function ($item) {
            $movementDebitBalance   = $item->journalTransactionDebit->sum('balance');
            $movementCreditBalance  = $item->journalTransactionCredit->sum('balance');
            $currentBalance         = $movementDebitBalance - $movementCreditBalance;
            return [
                'account_group_name'        => $item->accountGroup->name,
                'account_group_code'        => $item->accountGroup->code,
                'id'                        => $item->id,
                'name'                      => $item->name,
                'code'                      => $item->code,
                'opening_credit_balance'    => $item->opening_credit_balance,
                'opening_debit_balance'     => $item->opening_debit_balance,
                'movement_debit_balance'    => $movementDebitBalance,
                'movement_credit_balance'   => $movementCreditBalance,
                'current_balance'           => $currentBalance,
                'total'                     => $item->opening_debit_balance - $item->opening_credit_balance + $currentBalance,
            ];
        })->toArray();



public static function itemAktiva($year, $type)
    {
        $datas = self::getDataAktiva($year, $type);
        $totalDebitBalance          = 0;
        $totalCreditBalance         = 0;
        $totalOpeningBalance        = 0;
        $totalMovementDebitBalance  = 0;
        $totalMovementCreditBalance = 0;
        $totalCurrentBalance        = 0;

        // Menggunakan collect() untuk melakukan sum() pada kolom yang diinginkan
        $totalDebitBalance          = collect($datas)->sum('opening_debit_balance');
        $totalCreditBalance         = collect($datas)->sum('opening_credit_balance');
        $totalOpeningBalance        = $totalDebitBalance - $totalCreditBalance;
        $totalMovementDebitBalance  = collect($datas)->sum('movement_debit_balance');
        $totalMovementCreditBalance = collect($datas)->sum('movement_credit_balance');
        $totalCurrentBalance        = collect($datas)->sum('current_balance');
        $jumlah                     = $totalOpeningBalance + $totalCurrentBalance;

        $accountGroupName = collect($datas)->pluck('account_group_name')->filter()->first();
        $accountGroupCode = collect($datas)->pluck('account_group_code')->filter()->first();

        return [
            'account_group_name'    => $accountGroupName,
            'account_group_code'    => $accountGroupCode,
            'data'                  => $datas,
            'totalSaldoAwalDebet'   => $totalDebitBalance,
            'totalSaldoAwalKredit'  => $totalCreditBalance,
            'totalSaldoAwal'        => $totalOpeningBalance,
            'totalPergerakanDebet'  => $totalMovementDebitBalance,
            'totalPergerakanKredit' => $totalMovementCreditBalance,
            'totalSaldoBerjalan'    => $totalCurrentBalance,
            'jumlah'                => $jumlah,
        ];
    }
```
