# lib-mailer

Adalah library untuk memungkinkan aplikasi mengirim email. Library ini
di test dengan sender menggunakan akun google.

## Instalasi

Jalankan perintah di bawah di folder aplikasi:

```
mim app install lib-mailer
```

## Konfigurasi

Module ini membutuhkan tambahan konfigurasi pada level aplikasi sebagai berikuta:

```php
return [
    // ...
    'libMailer' => [
        'SMTP'          => true,
        'Host'          => 'smtp.gmail.com',
        'SMTPAuth'      => true,
        'Username'      => 'mail@gmail.com',
        'Password'      => '/secret/',
        'SMTPSecure'    => 'tls',
        'Port'          => 587,
        'FromEmail'     => 'mail@gmail.com',
        'FromName'      => 'My Name'
    ]
    // ...
];
```

## Penggunaan

Module ini menambahkan satu library dengan nama `LibMailer\Library\Mailer` yang
bisa digunakan untuk mengirim email. Library ini memiliki method sebagai berikut:

### send(array $options): bool

Contoh cara mengirim email dengan library ini adalah sebagai berikut:

```php
use LibMailer\Library\Mailer;

$options = [
    'to' => [
        [
            'name' => 'User One',
            'email' => 'user.one@gmail.com',
            'cc' => [   // optional
                [
                    'name' => 'user One One',
                    'email' => 'user.one.one@gmail.com'
                ]
            ],
            'bcc' => [
                [
                    'name' => 'user One Two',
                    'email' => 'user.one.two@gmail.com'
                ]
            ]
        ],
        [   // optional
            'name' => 'User Two',
            'email' => 'user.two@gmail.com'
        ]
    ],
    'subject' => 'Welcome to the club',
    'attachment' => [
        [
            'file' => '/absolute/path/to/file.ext',
            'name' => 'file.ext'
        ]
    ],
    'text' => '/Optional text if property view is not set/',
    'view' => [ // optional
        'path' => 'path/to/view',
        'params' => [
            'additional' => 'data',
            'to' => 'forward to',
            'view' => 'renderer'
        ]
    ]
];

if(!Mailer::send($options))
    die(Mailer::getError());
```

Keterangan masing-masing properti adalah sebagai berikut:

1. `to`  
   Array list penerima email, nilai ini juga akan diteruskan ke view
   jika menggunakan view renderer.
1. `subject`  
   String email subject, nilai ini menerima parameter seperti `(:email)` 
   yang akan di ganti dengan nilai to.email.
1. `text`  
   Konten body email jika email reader user tidak mendukung html. Nilai dari
   properti ini juga yang akan digunakan sebagai body email jika properti `view`
   tidak di set.
1. `view`  
   Adalah array yang berisi informasi view yang akan di render untuk membuat body
   html email. Properti ini memiliki dua sub properti, yaitu:

   1. `path`  
      Path ke view file tanpa extensi.
   1. `params`  
      Parameter key-value pair yang akan dikirim ke renderer view.

### getError(): ?string

## License

Module ini menggunakan class [PHPMailer](https://github.com/PHPMailer/PHPMailer).
Silahkan mengacu pada external class tersebut untuk lisensi.