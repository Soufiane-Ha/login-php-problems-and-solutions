# login-php-problems-and-solutions
Sessions and cookies: These are one way to save registration information such as username, ID, and email address.

>## Session 

Identification : It is a mechanism used to store user data on a server.

Time : During the user's stay on the site (short life).

Security : High security

Size :  /

>## Cookies 

Identification : These are small files that are stored on the user's device.

Time : The data is stored on the user's device and lasts for longer periods (depending on cookie settings).

Security : Less safe

Size : Cookies have a limited size (approximately 4KB per cookie).

>## Example :
```
<?php
session_start();

$pdo = new PDO("mysql:host=localhost;dbname=mydatabase", "username", "password");

if ($_SERVER['REQUEST_METHOD'] == 'POST') {

    $email = $_POST['email'];
    $password = $_POST['password'];

    $stmt = $pdo->prepare("SELECT * FROM users WHERE email = ?");
    $stmt->execute([$email]);

    $user = $stmt->fetch();

    if ($user && password_verify($password, $user['password'])) {

        $_SESSION['user_id'] = $user['id'];
        if (isset($_POST['remember_me'])) {
            setcookie('user_id', $user['id'], time() + (30 * 24 * 60 * 60), "/");
        }

        header("Location: dashboard.php");
        exit;
    } else {
        $error = "Incorrect email address or password.";
    }
}
?>

```
>## Form login :
```
   <form method="POST" action="">
        <label for="email">Email:</label>
        <input type="email" name="email" id="email" required>
        <br><br>

        <label for="password">Password:</label>
        <input type="password" name="password" id="password" required>
        <br><br>

        <label>
            <input type="checkbox" name="remember_me"> Remember me
        </label>
        <br><br>

        <button type="submit">Login</button>
    </form>
```
>## veiw errors :
```
    <?php if (isset($error)) echo "<p style='color:red;'>$error</p>"; ?>
```

>## End addition Logout :
```
<?php
session_start();

session_unset();
session_destroy();

if (isset($_COOKIE['user_id'])) {
    setcookie('user_id', '', time() - 3600, "/"); 
}

header("Location: login.php");
exit;
?>
```
