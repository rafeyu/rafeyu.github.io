---
layout: post
title: "Koneksi Database Menggunakan PDO"
modified: 2014-04-05 19:20:22 +0700
tags: [database,php]
category: coding
image:
  feature: 
  credit: 
  creditlink: 
comments: true
redirect_from: "/2014/04/koneksi-database-menggunakan-pdo/"
share: true
---

[PDO (PHP Data Objects)][1] adalah interface untuk mengakses database yang sudah disediakan oleh PHP untuk menggantikan fitur `mysql_`. Seperti kita tahu, dari dahulu kita diajarkan untuk menggunakan fitur `mysql_` dalam setiap bentuk koneksi database maupun melakukan operasi CRUD pada PHP, namun fitur ini sudah [*deprecated*][2].

Fitur `mysql_` yang saya sebutkan ini contohnya seperti fitur `mysql_connect`, `mysql_query`, dsb. Konyolnya masih banyak tutorial-tutorial di internet maupun maupun mata kuliah pemrograman web yang masih menggunakan cara *jadul* ini, cara yang sudah tidak direkomendasikan dalam melakukan interaksi dengan database pada PHP.

Selain PDO ada juga fitur mysqli, namun mengapa saya menuliskan PDO? Karena PDO bisa digunakan pada database apapun, berbeda dengan mysqli yang hanya bisa digunakan untuk database MySQL.

Untuk membuat koneksi awal dengan PDO pada database MySQL, sintak yang digunakan adalah

{% highlight php %}
$pdo = new PDO("mysql:host=$nama_host;dbname=$nama_database",$username_database,$password_database);
{% endhighlight %}

Untuk mengeksekusi perintah SQL menggunakan *prepared statement* maka sintak yang saya gunakan:

{% highlight php %}
$stmt = $pdo->prepare($sql);
{% endhighlight %}

Yang kemudian saya eksekusi dengan perintah `execute`.

{% highlight php %}
$stmt->execute($parameter);
{% endhighlight %}

Maka sintak lengkap yang saya gunakan untuk mengambil data user yang memiliki ID=1 adalah seperti ini:

{% highlight php %}
<?php
	try {
		$host = 'localhost';
		$db = 'user';
		$username = 'root';
		$password = '';
		$pdo = new PDO("mysql:host=$host;dbname=$db",$username,$password);

		$sql = 'SELECT * FROM user WHERE id = ?';
		$sth = $pdo->prepare($sql);
		$sth->execute(array(1));
		$hasil = $sth->fetchAll();
		foreach ($hasil as $h) {
			echo $h['username'];
		}
	} catch (PDOException $e) {
		echo $e->message;
	}
?>
{% endhighlight %}

[1]: http://www.php.net/manual/en/intro.pdo.php
[2]: https://wiki.php.net/rfc/mysql_deprecation
