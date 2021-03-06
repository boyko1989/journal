# Подписки

> #### Стоит ознакомиться:
>
> - [Как организовать процесс подписки на вашем сайте](https://habr.com/ru/company/unisender/blog/151265/)
> [Как создать подписку на обновления сайта](https://serpstat.com/ru/blog/kak-nastroit-podpisku-na-obnovlenija-sajta/)

## [Своя рассылка на PHP](https://htmlweb.ru/php/example/mail_add_and_send.php)

В интернете есть много бесплатных сервисов. В том числе и услуги по рассылке писем подписчикам. Некоторые из них очень хороши. Но раз уж они бесплатны, значит тут же в письмах появляется реклама. Да и немалые формы для подписки с логотипом представителя услуг многих не устраивают.

Так почему бы не написать простенький движок для своей рассылки и отсылать письма своими силами?

Давайте разберемся, что нам для этого потребуется. Для того чтоб посетители имели возможность подписаться на вашу рассылку необходима форма для ввода адреса электронной почты. После ввода адрес надо запомнить. Давайте адреса будем сохранять в файле maillist.txt по одному адресу в строчке. После того как адрес будет сохранен, давайте выведем соответствующее сообщение и отобразим форму для подписки еще одного адреса или удаления существующего. Вот собственно почти и все. Осталось добавить только возможность отправки писем. Для безопастности, давайте на возможность отправки писем поставим пароль - необходима форма для ввода пароля. Далие потребуются формы для заполнения адреса отправителя и темы, а также для самого текста. Ну и, наконец, сам скрипт, который будет отсылать письма. А теперь все по порядку.

Форма для ввода адреса электронной почты будет состоять только из окна для ввода адреса и кнопки для подтверждения:
```html
<form method="post" action="ras.php" enctype="multipart/form-data">
    <input type="text" name="email" size="30">
    <input type="submit" name="submit" value="подписаться"> 
</form> 
```
Итак, в окне для ввода текста, с именем email и видимой длиной в 30 символов, будет вводиться адрес электронной почты. После нажатия на кнопку с надписью подписаться, адрес будет передан скрипту ras.php для занесения в базу рассылки.

Далее давайте рассмотрим скрипт ras.php который будет сохранять адрес почты в файле, выводить сообщение о результате и формы для подписки и отписки. Скрипт можно исполнить совсем просто - сохранить адрес, вывести соответствующее сообщение. Но могут возникнуть проблемы: кто-то может случайно подписаться несколько раз, кто-то может допустить опечатку и ввести в поле адреса недопустимый символ. В таком случае база рассылки будет загрязняться, а неверные адреса станут приводить к ошибкам в работе скрипта. Вывод ясен - перед сохранением адреса следует проверить его на соответствие стандартам имен адресов электронных почтовых ящиков, а также на наличие в базе рассылки. Для того чтоб не рассматривать код по частям, я дам комментарии в самом коде:

```php
$file = "maillist.txt"; // файл, содержащий адреса

error_reporting(0); // запрещаем вывод сообщений о возможных ошибках
function test_mail($char) { // функция, проверяющая реальность адреса
   if (preg_match("/^[_\.0-9a-z-]+@([0-9a-z][-0-9a-z\.]+)\.([a-z]{2,3}$)/", $char)) return true;
   return false;
}

// получаем введеный в форму адрес с символами в нижнем регистре
$email = trim(strtolower($email));

function copy_mail($char){ // проверяем, есть ли такой адрес в базе
   global $file;
   $list = file($file);
   for ($i = 0; $i < sizeof ($list); $i++)
    if ($char == trim($list[$i])) return true;
   return false;
}

echo "<center>";

if (is_file($file)) {// далее проверяем адрес вышеописаными функциями

    $maillist = file($file);
    if (!$email == '') {
        if (test_mail($email)) {
            if (!copy_mail($email)){
                $maillist[] = "\n$email";
                print "E-mail: $email добавлен базу рассылки</center>";
            } else print "E-mail: $email уже есть в базе</center>";
        } else print "E-mail: $email не сушествует</center>";
    } else print "</center>";
} else print "Не найден файл $file ! Пожалуйста <A HREF="mailto:$fromemail">сообщите</a> мне о ошибке.</center>";

// выводим на экран форму с предложением подписки и отписки

echo '<br><center>Подписаться на рассылку
        <form method="post" action="ras.php" enctype="multipart/form-data">
            <label>Введите mail:</label><br>
            <input type="text" name="email" size="30">
            <input type="submit" name="submit" value="подписаться">
        </form></center>
        <center><br><br>
        <form method="post" action="ras.php" enctype="multipart/form-data">
            <label>Отписаться от рассылки<br>Введите mail:</label>
            <input type="text" name="delmail" size="15">
            <input type="submit" name="submit" value="Отписаться">
        </form></center>';

// если пользователь решил отписаться - удаляем введеный адрес
$flag = false;
$fw = fopen($file, "w");
for ($i = 0; $i < sizeof ($maillist); $i++) {
    if (trim(strtolower($delmail)) == trim(strtolower($maillist[$i]))) {
        if (!$delmail == '') {
            print "<center>$delmail удален из базы рассылки</center>";
            $flag = true;
        }
    } else fputs($fw, $maillist[$i]); // введеного адреса в базе нет
}
/* Нужно при практической работе проверить - скорее всего здесь допущена ошибка */   
fclose($fw);
if (!$delmail == '')
if (!$flag) print "<center>$delmail не найден в базе рассылки</center>";
```
Вот наш код сохранения и удаления адресов готов. Теперь надо позаботится о средствах отправки почты. Не будем же мы через Outlook отсылать!?. Как уже говорилось, защитим возможность отправки паролем, который будем вводить на специальной форме:
```html
<form method="POST" action="out.php"> 
    <input type="password" name="pass" value=""> 
    <input type="submit" value="войти"> 
</form> 
```
Поле для ввода с именем pass и будет служить для ввода пароля. После нажатия на кнопку с надписью войти, пароль будет передан скрипту out.php:
```php
$subject = "Рассылка моего сайта"; // тема рассылки
$fromemail = "мое@мыло"; // ваш адрес (для ответов)
$file = "maillist.txt"; // список адресов подписчиков
$password = "secretpassword"; // ваш пароль для рассылки

if ($_POST['pass'] == $password){ // если пароль ввели правильный
// то выводим форму с полями для ввода:
// адрес отправителя, текст письма, тело письма
// кнопку для отправления
// после нажатия на кнопку, передаем данные скрипту send.php
echo '<font size="-1"><hr>
    <form method="POST" action="send.php">
        <label>адрес отправителя</label><br>
        <input type="text" name="fromemail" value="$fromemail" size="25"><br>
        <label>тема письма</label><br><input type="text" name="subject" value="$subject" size="50"><br>
        <label>текст письма:</label><br>
        <textarea name="body" rows="8" cols="50"></textarea><br>
        <input type="submit" value="Отправить сообщение">
    </form></font>
    <i>В базе<b>'.sizeof($maillist).'</b> адресов</i><br><hr>';

    for ($i = 0; $i < sizeof ($maillist); $i++) print $maillist[$i]. "<br>";
} else // если пароль неверный - просим ввести еще раз
echo '<form method="POST" action="ras.php">
        <input type="password" name="pass" value="">
        <input type="submit" value="Управление">
    </form>';
```
Осталось рассмотреть только один скрипт - тот самый, который будет отсылать почту:
```php
$odr = "\n\n\n Для отказа от подписки воспользуйтесь ссылкой\n"; 
$homepage = "http://адрес.сайта/ras.php"; error_reporting(0); 
$subject = $HTTP_POST_VARS["subject"]; 
$body = $HTTP_POST_VARS["body"]; 
$subject = stripslashes($subject); 
$body = stripslashes($body); 
$file = "maillist.txt"; 
$maillist = file($file); 
print "В базе". sizeof($maillist) ." адресов<br>"; 
for ($i = 0; $i < sizeof ($maillist); $i++{ 
    #echo($maillist[$i]."<br>"); 

    /* Нужно при практической работе проверить - скорее всего здесь допущена ошибка */  

    mail($maillist[$i], $subject, $body ."$odr $homepag?delmail=$maillist[$i]", "From: <$fromemail>");
} 

echo "Готово!";
```
Тут все просто: получаем значения, введенные в предыдущюю форму и в цикле отправляем их по очереди на каждый из адресов.

