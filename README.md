<?php

include_once 'include/config.php';

class User {


public function __construct() 
{
        $db = new DB_Class();
}
    
   
public function register_user($fullname, $username, $email, $mobile, $orgname, $password) 
{
        $password = md5($password);
        
        $sql = mysql_query("SELECT uid from users WHERE username = '$username' or email = '$email'");
        $no_rows = mysql_num_rows($sql);
        
        if ($no_rows == 0) 
        {
        $result = mysql_query("INSERT INTO users(fullname, username, email, mobile, orgname, password) values ('$fullname', '$username','$email','$mobile','$orgname', '$password')") or die(mysql_error());
        return $result;
        }
        else
        {
        return FALSE;
        }
        
    }

   public function check_login($username, $password) 
    {
        $password = md5($password);
        
        $result = mysql_query("SELECT uid from users WHERE email = '$username' or username='$username' and password = '$password'");
        $user_data = mysql_fetch_array($result);
        $no_rows = mysql_num_rows($result);
        
        if ($no_rows == 1) 
        {
     
            $_SESSION['login'] = true;
            $_SESSION['uid'] = $user_data['uid'];
            return TRUE;
        }
        else
        {
            return FALSE;
        }
    }

    public function get_fullname($uid) 
    {
        $result = mysql_query("SELECT fullname FROM users WHERE uid = $uid");
        $user_data = mysql_fetch_array($result);
        echo $user_data['fullname'];
    }
  


    public function get_session() 
    {
        
        return $_SESSION['login'];
    }


 // if {
 //    (isset($_SESSION['login']))

 //    {

    public function user_logout() {
        $_SESSION['login'] = FALSE;
        session_destroy();
    }

}

?>
