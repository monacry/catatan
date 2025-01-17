```php
public static function testing()
{
    $dataAccountGroup = [
        // Group Saldo
        ["code" => 111000, "name" => "Kas",     "slug" => "111000", "year" => 2024, "user_id" => 1, "created_by_id" => 1, "created_at" => now(), "updated_at" => now()],
        ["code" => 112000, "name" => "Bank",    "slug" => "112000", "year" => 2024, "user_id" => 1, "created_by_id" => 1, "created_at" => now(), "updated_at" => now()],
    ];
    // AccountGroup::insert($dataAccountGroup);
    $dataAccountCodes  = array_column($dataAccountGroup, 'code');
    $accountCode       = AccountGroup::yearOwnership(2024)->whereIn('code', $dataAccountCodes)->pluck('id', 'code');
    $dataAccountGroupUpdate = [
        // Group Saldo
        ["id" => $accountCode[111000], "code" => 111000, "name" => "ssssss",   "slug" => "111", "year" => 2024, "user_id" => 1, "created_by_id" => 1, "created_at" => now(), "updated_at" => now()],
        ["id" => $accountCode[112000], "code" => 112000, "name" => "123123",  "slug" => "222", "year" => 2024, "user_id" => 1, "created_by_id" => 1, "created_at" => now(), "updated_at" => now()],
    ];
    AccountGroup::upsert($dataAccountGroupUpdate, ['id'], ['slug']);
}
```
