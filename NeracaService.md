```blade
        // return $query->map(function ($item) {
        //     $movementDebitBalance   = $item->journalTransactionDebit->sum('balance');
        //     $movementCreditBalance  = $item->journalTransactionCredit->sum('balance');
        //     $currentBalance         = $movementDebitBalance - $movementCreditBalance;
        //     return [
        //         'account_group_name'        => $item->accountGroup->name,
        //         'account_group_code'        => $item->accountGroup->code,
        //         'id'                        => $item->id,
        //         'name'                      => $item->name,
        //         'code'                      => $item->code,
        //         'opening_credit_balance'    => $item->opening_credit_balance,
        //         'opening_debit_balance'     => $item->opening_debit_balance,
        //         'movement_debit_balance'    => $movementDebitBalance,
        //         'movement_credit_balance'   => $movementCreditBalance,
        //         'current_balance'           => $currentBalance,
        //         'total'                     => $item->opening_debit_balance - $item->opening_credit_balance + $currentBalance,
        //     ];
        // })->toArray();
```
