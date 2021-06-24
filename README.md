# Password Generator/Hash/Match Script

Tiny PHP Script to Generate/Hash/Match a Random Password. the Length is always multiplied by 2.

```php

<?php

function generatePassword($length=32) {
    $symbols = [];

    if (!is_numeric($length) && $length > 1)
        return 'Invalid Length !';

    // Generate Random Strong Password (PHP Version 7.*)
    $symbols["upper_case"] = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ';
    $symbols["special_symbols"] = '?~@#-_+[]{}';

    $sPassword = bin2hex(random_bytes(($length)));
    for ($i=0;$i<($length/2);$i++) {

        $rPos = random_int(0, (strlen($sPassword)));
        $rUCase = $symbols["upper_case"][random_int(0,(strlen($symbols["upper_case"])-1))];
        $sPassword = substr($sPassword, 0, ($rPos-1)) . $rUCase . substr($sPassword, $rPos);

        $rPos = random_int(0, (strlen($sPassword)));
        $rSSymbol = $symbols["special_symbols"][random_int(0, (strlen($symbols["special_symbols"])-1))];
        $sPassword = substr($sPassword, 0, ($rPos-1)) . $rSSymbol  . substr($sPassword, $rPos);
    }

    return $sPassword;
}

function hashPassword($password)
{
    if (empty($password) || gettype($password) !== 'string')
        return false;
    
    return password_hash($password, PASSWORD_DEFAULT);
}

function matchPassword($password, $hash)
{
    if (
        empty($password) || gettype($password) !== 'string'
        empty($hash) || gettype($hash) !== 'string'
    )
        return false;
    
    return password_verify($password, $hash) ? true : false;
}
```
