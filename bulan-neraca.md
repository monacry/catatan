```php
$modelAccount       = Account::with([
            'journalDebit' => function ($query) use ($selectedMonth) {
                $query->where(function ($subQuery) use ($selectedMonth) {
                    foreach ($selectedMonth as $month) {
                        $subQuery->orWhereMonth('date', $month);
                    }
                });
            },
            'journalCredit' => function ($query) use ($selectedMonth) {
                $query->where(function ($subQuery) use ($selectedMonth) {
                    foreach ($selectedMonth as $month) {
                        $subQuery->orWhereMonth('date', $month);
                    }
                });
            },
            'accountGroup'  => function ($query) use ($listTypeActive) {
                $query->where('group_type', 'balance-sheet')->whereIn('account_type', $listTypeActive);
            },
            'setAs'
        ])
            ->whereYearOwnership($user->id, $year)
            ->whereRelation('accountGroup', function ($query) use ($listTypeActive) {
                $query->where('group_type', 'balance-sheet')->whereIn('account_type', $listTypeActive);
            })
            ->whereRelation('journalDebit', function ($query) use ($selectedMonth) {
                $query->where(function ($subQuery) use ($selectedMonth) {
                    foreach ($selectedMonth as $month) {
                        $subQuery->orWhereMonth('date', $month);
                    }
                });
            })
            ->whereRelation('journalCredit', function ($query) use ($selectedMonth) {
                $query->where(function ($subQuery) use ($selectedMonth) {
                    foreach ($selectedMonth as $month) {
                        $subQuery->orWhereMonth('date', $month);
                    }
                });
            })
            ->orderBy('code')
            ->get();
```
