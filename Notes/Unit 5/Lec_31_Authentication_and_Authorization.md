# Lec 31: Authentication & Authorization

**Subject:** Operating Systems\
**Lecture Duration:** 50 minutes\
**Audience:** Undergraduate students

------------------------------------------------------------------------

## Learning Outcomes

By the end of this lecture, you should be able to:

-   Differentiate authentication and authorization.
-   Explain how password-based authentication works.
-   Describe common password attacks and safe password practices.
-   Explain Multi-Factor Authentication (MFA) and its factor categories.
-   Connect authentication and authorization to operating-system
    security.

## 1. Introduction

Operating systems support multiple users, applications, and resources.
Before granting access, a system needs to establish **who is making the
request** and decide **what that identity is allowed to do**.

Example: When you sign in to a college portal, you enter your username
and password. The portal checks your identity (authentication). After
sign-in, it checks whether your account is allowed to view marks, edit
attendance, or manage users (authorization).

``` mermaid
flowchart TD
    A["User requests access"] --> B["Authentication<br/>Who are you?"]
    B --> C{"Identity verified?"}
    C -- No --> D["Reject sign-in"]
    C -- Yes --> E["Authorization<br/>What are you allowed to do?"]
    E --> F["Allow permitted actions"]
    E --> G["Deny restricted actions"]
    style B fill:#dbeafe,stroke:#2563eb,color:#222
    style E fill:#d1fae5,stroke:#047857,color:#222
    style D fill:#f8caca,stroke:#b42318,color:#222
    style G fill:#f8caca,stroke:#b42318,color:#222
```

------------------------------------------------------------------------

## 2. Authentication vs Authorization

### 2.1 Authentication

**Authentication** is the process of verifying that a user, device, or
process is who or what it claims to be.

It answers:

> **Who are you?**

Common examples: - Entering a username and password. - Unlocking a phone
with a fingerprint. - Using a security key. - Entering a one-time code
as an additional verification step.

### 2.2 Authorization

**Authorization** is the process of deciding which resources or actions
an authenticated identity is permitted to access.

It answers:

> **What are you allowed to do?**

Common examples: - A student can view marks but cannot edit them. - A
teacher can enter marks for assigned classes. - An administrator can
create accounts and manage system settings. - A Linux user can read a
file but cannot modify it.

### 2.3 Comparison Table

  -----------------------------------------------------------------------
  Basis                   Authentication          Authorization
  ----------------------- ----------------------- -----------------------
  Main question           Who are you?            What may you access or
                                                  do?

  Purpose                 Verify identity         Enforce access
                                                  permissions

  Usually happens         Before access is        After identity is
                          granted                 established, when a
                                                  resource/action is
                                                  requested

  Example                 Sign in using a         Permit the user to
                          password                view, but not edit,
                                                  marks

  Failure result          Sign-in may be rejected A specific action or
                                                  resource request is
                                                  denied
  -----------------------------------------------------------------------

**Important:** Authentication and authorization are different, but both
are necessary. A successfully authenticated user should not
automatically receive administrator privileges.

### Real-life analogy: College ID

-   **Authentication:** A guard checks that your college ID belongs to
    you.
-   **Authorization:** The ID or access system determines whether you
    can enter the library, laboratory, staff room, or server room.

------------------------------------------------------------------------

## 3. Password-Based Authentication

Password-based authentication verifies a user by checking a secret
password associated with an account.

### Typical sign-in flow

``` mermaid
flowchart TD
    A["User enters username and password"] --> B["System identifies the account"]
    B --> C["Verify password against stored verifier"]
    C --> D{"Match?"}
    D -- Yes --> E["Authentication succeeds"]
    D -- No --> F["Authentication fails"]
    E --> G["Continue to authorization checks"]
    style E fill:#d1fae5,stroke:#047857,color:#222
    style F fill:#f8caca,stroke:#b42318,color:#222
```

In a well-designed system, the actual password is **not stored as plain
text**. Instead, the system stores a password verifier derived using a
password-hashing algorithm and a unique salt. During sign-in, it uses
the submitted password to calculate and compare the verifier.

### Password storage: unsafe vs safer

``` text
Unsafe approach:
Database stores:  myPassword123
                       |
                       v
             Anyone who obtains the
             database may read passwords

Safer approach:
Password + unique salt
          |
          v
Password-hashing algorithm
(e.g., Argon2id, scrypt, bcrypt, or PBKDF2)
          |
          v
Stored salt + password hash
```

A **salt** is a random value stored alongside the password hash. It
helps prevent attackers from using precomputed tables and ensures that
identical passwords do not automatically produce identical stored hashes
when different salts are used.

Password hashing is deliberately designed to make guessing expensive. A
fast general-purpose hash such as plain SHA-256 is not suitable by
itself for password storage.

### What makes a password stronger?

-   Use long, unique passwords or passphrases.
-   Never reuse an important password across multiple websites.
-   Consider a reputable password manager.
-   Avoid predictable information such as names, birthdays, or common
    patterns.
-   Enable MFA where possible.
-   Change a password promptly if it is exposed or suspected to be
    compromised.

Example of a passphrase style:

``` text
River-Coffee-Window-Planet-47
```

This is only an illustrative pattern, not a password to use as-is. A
password manager can generate and store a unique random password for
each account.

### Common password-related attacks

  -----------------------------------------------------------------------
  Attack                  Meaning                 Basic defence
  ----------------------- ----------------------- -----------------------
  Brute force             Tries many possible     Rate limits,
                          passwords               throttling, MFA, strong
                                                  passwords

  Dictionary attack       Tries common words and  Long, uncommon
                          known password patterns passwords; block common
                                                  passwords

  Credential stuffing     Uses                    Unique passwords, MFA,
                          usernames/passwords     monitoring
                          leaked from other       
                          services                

  Phishing                Tricks a user into      Verify websites, avoid
                          revealing credentials   suspicious links, use
                                                  phishing-resistant MFA

  Shoulder surfing        Observes a password     Shield the screen and
                          being entered           keyboard; avoid exposed
                                                  sign-in areas
  -----------------------------------------------------------------------

Systems should use sensible rate limiting and monitoring. Lockout
policies must be designed carefully so that attackers cannot easily lock
other users out.

------------------------------------------------------------------------

## 4. Multi-Factor Authentication (MFA)

**Multi-Factor Authentication (MFA)** requires two or more independent
types of evidence to verify a user's identity.

It reduces the risk that a stolen password alone will be enough to
access an account.

### The three main factor categories

1.  **Something you know** --- a password, PIN, or passphrase.
2.  **Something you have** --- a registered phone, hardware security
    key, or authenticator device.
3.  **Something you are** --- a biometric characteristic, such as a
    fingerprint or face scan.

``` mermaid
flowchart TD
    A["Authentication factors"] --> B["Know"]
    A --> C["Have"]
    A --> D["Are"]
    B --> E["Password or PIN"]
    C --> F["Security key or authenticator device"]
    D --> G["Fingerprint or face recognition"]
    style B fill:#dbeafe,stroke:#2563eb,color:#222
    style C fill:#d1fae5,stroke:#047857,color:#222
    style D fill:#fef3c7,stroke:#b45309,color:#222
```

**Important distinction:** Two passwords are still two instances of the
same factor category, not true multi-factor authentication. MFA combines
different categories.

### Example: Password + authenticator code

1.  The user enters a password.
2.  The system verifies the password.
3.  The user supplies a time-based code from an authenticator app or
    approves a sign-in request.
4.  The system verifies the second factor.
5.  Access is granted if all required checks pass.

``` mermaid
flowchart TD
    A["Enter password"] --> B{"Password valid?"}
    B -- No --> X["Deny access"]
    B -- Yes --> C["Verify second factor"]
    C --> D{"Second factor valid?"}
    D -- No --> X
    D -- Yes --> E["Authentication succeeds"]
    style E fill:#d1fae5,stroke:#047857,color:#222
    style X fill:#f8caca,stroke:#b42318,color:#222
```

### Common MFA methods

  -----------------------------------------------------------------------
  Method                  Factor category         Notes
  ----------------------- ----------------------- -----------------------
  Authenticator-app code  Have                    Better than password
                                                  alone, but codes can
                                                  still be phished

  SMS one-time code       Have (phone access)     Better than no second
                                                  factor, but vulnerable
                                                  to SIM-swap and
                                                  interception risks

  Hardware security key / Have, often with local  Phishing-resistant
  passkey                 user verification       options are strongly
                                                  preferred where
                                                  available

  Fingerprint or face     Are                     Often unlocks a
  recognition                                     device-held credential;
                                                  biometric data should
                                                  be handled carefully

  Push approval           Have                    Users should reject
                                                  unexpected prompts;
                                                  number matching can
                                                  reduce approval fatigue
  -----------------------------------------------------------------------

MFA methods differ in strength. Phishing-resistant methods, such as
FIDO2 security keys and passkeys, are generally stronger against fake
sign-in pages than codes that users manually type into a website.

### Advantages of MFA

-   Makes stolen passwords less useful to attackers.
-   Adds another barrier to unauthorized access.
-   Can reduce the impact of password reuse and credential leaks.

### Limitations of MFA

-   Users may lose a phone or security key.
-   SMS codes can be intercepted or redirected.
-   Users may approve fraudulent push requests.
-   Account recovery can become a weak point if not protected properly.

**Best practice:** Enable MFA, prefer phishing-resistant methods when
available, protect recovery options, and never approve an unexpected
sign-in prompt.

------------------------------------------------------------------------

## 5. Authentication and Authorization in an Operating System

An operating system uses authentication and authorization as parts of
its overall access-control model.

### Linux example

When a user signs in to a Linux system, the system authenticates the
account. After sign-in, the kernel and system services enforce
permissions when the user accesses files, devices, or other resources.

``` bash
whoami
```

Displays the current username.

``` bash
id
```

Displays the current user ID (UID), group ID (GID), and group
memberships.

``` bash
ls -l marks.txt
```

Shows file ownership and basic permissions.

Example:

``` text
-rw-r----- 1 akash faculty 2048 Oct 26 09:00 marks.txt
```

Simplified interpretation: - Owner (`akash`): read and write. - Group
(`faculty`): read. - Others: no access.

These permissions are an authorization mechanism. They do not, by
themselves, prove how the user authenticated.

**Safety note:** Do not practise by changing permissions on important
system files. Use a temporary test file in your own directory.

------------------------------------------------------------------------

## 6. Quick Revision

-   **Authentication:** Verifies identity --- "Who are you?"
-   **Authorization:** Determines permissions --- "What can you do?"
-   **Password authentication:** Checks a submitted secret against a
    safely stored password verifier.
-   **Salt:** A unique random value used when deriving a stored password
    hash.
-   **MFA:** Uses at least two different factor categories.
-   **Know:** Password or PIN.
-   **Have:** Security key or authenticator device.
-   **Are:** Fingerprint or face recognition.
-   **Good practice:** Use unique passwords, enable MFA, and prefer
    phishing-resistant methods.

------------------------------------------------------------------------

## 7. Practice Questions

### Short-answer questions

1.  Define authentication.
2.  Define authorization.
3.  Differentiate authentication and authorization with one example
    each.
4.  Explain how password-based authentication works.
5.  Why should systems avoid storing passwords in plain text?
6.  What is a salt in password storage?
7.  What is Multi-Factor Authentication (MFA)?
8.  List the three main authentication factor categories with examples.

### Application questions

1.  A student signs in successfully but tries to edit marks without
    permission. Which process should deny the action?
2.  A person obtains a user's password but cannot sign in because a
    second factor is required. Which security measure is being used?
3.  Why is using the same password on several websites dangerous?
4.  Explain why a password and a PIN do not usually count as two
    different MFA factors.
5.  Which is generally more resistant to phishing: a manually typed
    one-time code or a FIDO2 security key/passkey? Briefly explain.

### Self-check answers

1.  Authentication.
2.  Authorization.
3.  MFA.
4.  Both are "something you know," so they belong to the same factor
    category.
5.  A FIDO2 security key/passkey is generally more phishing-resistant
    because it is designed to bind authentication to the legitimate site
    or service.

------------------------------------------------------------------------

## Key Takeaway

**Authentication verifies identity. Authorization controls permissions.
Passwords are one way to authenticate, while MFA strengthens sign-in by
combining different types of evidence.**
