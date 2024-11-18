<!DOCTYPE html>
<html lang="en">

<head>
    <title>Sign Up Page</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
            background-image: url('background.jpg');
            background-repeat: no-repeat;
            background-attachment: fixed;
            background-size: cover;
        }

        .signup-container {
            width: 400px;
            margin: 40px auto;
            padding: 20px;
            background-color: #fff;
            border: 1px solid #ddd;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
        }

        .signup-form {
            display: flex;
            flex-direction: column;
            align-items: center;
        }

        h2 {
            margin-top: 0;
        }

        label {
            margin-bottom: 10px;
        }

        input[type="text"],
        input[type="email"],
        input[type="password"],
        input[type="date"] {
            width: 100%;
            height: 40px;
            margin-bottom: 20px;
            padding: 10px;
            box-sizing: border-box;
        }

        #signup {
            width: 100%;
            height: 40px;
            background-color: #333;
            color: #fff;
            border: none;
            cursor: pointer;
        }

        #signup:hover {
            background-color: #555;
        }

        a {
            text-decoration: none;
            color: #337ab7;
        }

        a:hover {
            color: #23527c;
        }

        #invalid {
            text-align: center;
            color: #FF0000;
            margin-bottom: 10px;
        }
    </style>
</head>

<body>
    <div class="signup-container">
        <div class="signup-form">
            <h2>Sign Up</h2>
            <form method="POST">
                <label for="UID">User ID:</label>
                <input type="text" id="UID" name="UID" required>
                <label for="DOB">Date Of Birth:</label>
                <input type="date" id="DOB" name="DOB" required>
                <label for="email">Email:</label>
                <input type="email" id="email" name="email" required>
                <label for="password">Password:</label>
                <input type="password" id="password" name="password" required>
                <label for="confirm-password">confirm-password:</label>
                <input type="password" id="confirm-password" name="confirm-password" required>
                <input type="submit" value="Sign Up" id="signup" name="signup">
                <?php
                if (isset($_POST['signup'])) {
                    $connectionInfo = array(
                        "UID" => "vandan",
                        "pwd" => "Naman@123",
                        "Database" => "management",
                        "LoginTimeout" => 30,
                        "Encrypt" => 1,
                        "TrustServerCertificate" => 0
                    );
                    $serverName = "tcp:manage.database.windows.net,1433";
                    $con = sqlsrv_connect($serverName, $connectionInfo);

                    if ($con === false) {
                        die("<p id='invalid'>Connection error: " . print_r(sqlsrv_errors(), true) . "</p>");
                    } else {
                        $uid = $_POST['UID'];
                        $DOB = $_POST['DOB'];
                        $email = $_POST['email'];
                        $password = $_POST['password'];
                        $c_password = $_POST['confirm-password'];

                        if ($password === $c_password) {
                            $query = "INSERT INTO login (username, DOB, email, password) VALUES (?, ?, ?, ?)";
                            $params = array($uid, $DOB, $email, $password);
                            $stmt = sqlsrv_query($con, $query, $params);

                            if ($stmt) {
                               echo "<script>window.location.href='dashboard.php';</script>";
                                exit;
                            } else {
                                echo "<p id='invalid'>Error: Could not execute query.</p>";
                            }
                        } else {
                            echo "<p id='invalid'>Password does not match</p>";
                        }

                        sqlsrv_close($con);
                    }
                }
                ?>
            </form>
            <p>Already have an account? <a href="login.php">Login</a></p>
        </div>
    </div>
</body>

</html>
