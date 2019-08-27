# dumbster
> The Dumbster is a very simple fake SMTP server designed for unit and system testing applications that
> send email messages. It responds to all standard SMTP commands but does not deliver messages to the user.
> The messages are stored within the Dumbster for later extraction and verification.
>
> **This repository is a fork of [https://github.com/kirviq/dumbster](https://github.com/kirviq/dumbster).**
>

I forked it with following changes:

1. Removed dependency on Project Lombok, Apache Felix.
2. Upgrade Java compile level to Java 11.
3. Changed mail encoding from ISO_8859_1 to UTF_8.
4. Added method to get user's mail box.
5. Added getSubject() to SmtpMessage.java

### Usage
Add maven dependency:
```xml
<dependency>
    <groupId>com.github.dqbai</groupId>
    <artifactId>dumbster</artifactId>
    <version>1.8</version>
    <scope>test</scope>
</dependency>
```
Start testing:
```java
class SomeTest {
    public void runTest() {
        try (SimpleSmtpServer dumbster = SimpleSmtpServer.start(SimpleSmtpServer.AUTO_SMTP_PORT)) {
        
            sendMessage(dumbster.getPort(), "sender@here.com", "Test", "Test Body", "receiver@there.com");
            
            List<SmtpMessage> emails = dumbster.getReceivedEmails();
            assertThat(emails, hasSize(1));
            SmtpMessage email = emails.get(0);
            assertThat(email.getHeaderValue("Subject"), is("Test"));
            assertThat(email.getBody(), is("Test Body"));
            assertThat(email.getHeaderValue("To"), is("receiver@there.com"));
        }
    }
}
```
See more examples in the included [unit tests](https://github.com/dqbai/dumbster/blob/master/src/test/java/com/dumbster/smtp/SimpleSmtpServerTest.java).
