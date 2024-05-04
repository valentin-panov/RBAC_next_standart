# RBAC in React/Next application

Applications developed using React often encounter security vulnerabilities, particularly concerning the limitation of unauthorized access. Here are several reasons why this occurs:

1. **Improper client-side data handling:** Developers may be tempted to store confidential data or access logic on the client side of the application, rendering them vulnerable to data interception attacks or exploitation through developer tools.

2. **Inadequate authentication and authorization:** Implementing incomplete or weak authentication and authorization can lead to users gaining access to sensitive data or functionalities they should not have rights to.

3. **Insufficient route protection:** If application routes are not adequately protected, malicious actors can discover and exploit vulnerable entry points to access protected resources.

We have developed a demonstration application where vulnerabilities have been implemented in the insufficient protection of routes. Specifically, the attacker's browser receives all JavaScript chunks that could be utilized on the page, and only after receiving this code does the browser decide to restrict access. As a result, the attacker gains access to all code, sometimes even code not intended for authenticated users but without administrator rights.

You may attempt to reproduce this behavior by entering the URL /admin in the address bar. For a brief moment, the browser will display the Admin Space page, only redirecting you to the authentication page thereafter.

The code responsible for such behavior may appears as follows:
```
  useEffect(() => {
    const token = verifyToken(getCookie("access_token"));
    if (!token) {
      window.location.href = "/";
    }
  }, []);
```

Similarly, if the 'customer' session is activated and a request is made to the main page, the browser will receive code for a component that should only be accessible to users with the "admin" role. This can be easily discerned by the presence of a JavaScript file in the developer tools containing the flag "FIND_ME_Users".

Furthermore, if a malicious actor alters the session role variable in the browser, which is not an insurmountable task, the browser will render a component that was originally not intended for a low-privileged user.

Methods for combating such behavior and making your application resilient against Broken Access Control attacks will be explored in upcoming publications. Stay tuned.

### Web version
https://rbac-next-standart.vercel.app/

### Used fake API:
https://fakeapi.platzi.com/en/rest/users/
