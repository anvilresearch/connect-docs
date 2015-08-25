## Password reset

By default, when a [mailer](mailer.md) is configured, and the [password provider]
(../providers/Password.md) is enabled, Anvil Connect will enable the ability to
recover a user's account by resetting their password.

When an e-mail is sent to a user, the password reset code in the given URL will
expire in **one day**.

Additionally, when a user's password is reset in this manner, they will receive
an e-mail that informs them that their password was changed, with a link to reset
their password in the event it was not them who did it.

### UX

1. User visits sign-in page
2. User clicks "Forgot password?"
3. User enters their e-mail address
4. User checks their e-mail account for the message with the recovery link
5. User clicks the recovery link
6. User enters and confirms their new password
7. User can now sign in with their new password

### E-mail templates
File | Description
---- | -----------
resetPassword.hogan | Provides tokenized link to reset password
passwordChanged.hogan | Informs user that password was changed

### Views
File | Description
---- | -----------
recovery/start.jade | User enters their e-mail address to begin the recovery process
recovery/emailSent.jade | Informs user that e-mail with reset link was sent
recovery/resetPassword.jade | User enters and confirms their new password
recovery/passwordReset.jade | Informs user that password was reset
