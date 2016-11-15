Send mail to
========================
2016-11-15


A simple send mail (wrapping SwiftMailer) function to copy paste in your php projects.




Install
--------------

Make sure SwiftMailer is installed.

Then copy this function in your function library, and read the comments
for documentation.





```php

/**
 *
 * This function simplify the sending of a simple email.
 * It depends on SwiftMailer.
 *
 * Also, you need to define those constants before you call the function:
 *
 * - MAIL_HOST
 * - MAIL_PORT
 * - MAIL_USER
 * - MAIL_PASS
 * - MAIL_FROM
 *
 *
 * - subject: string
 * - msg: string | array (0 => plainMsg, 1 => htmlMsg)
 *          if it's a string, only the plain message will be sent
 * - to: string | array, accept whatever SwiftMailer's to field accepts:
 *          More info here: http://swiftmailer.org/docs/sending.html
 *
 *
 */
function sendMailTo($subject, $msg, $to)
{
    $msgHtml = null;
    if (is_array($msg)) {
        $msg = $msg[0];
        $msgHtml = $msg[1];
    }

    $transport = Swift_SmtpTransport::newInstance(MAIL_HOST, MAIL_PORT)
        ->setUsername(MAIL_USER)
        ->setPassword(MAIL_PASS)
        ->setEncryption("ssl");
    $mailer = Swift_Mailer::newInstance($transport);

    // Create a message
    $message = Swift_Message::newInstance($subject)
        ->setFrom(MAIL_FROM)
        ->setTo($to)
        ->setBody($msg, 'text/plain');


    if (null !== $msgHtml) {
        $message->addPart($msgHtml, 'text/html');
    }
    $result = $mailer->send($message);
    return $result;
}

```


