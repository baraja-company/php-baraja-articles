Safe application
================

> id: cc4667a3-aea3-4e0a-b323-da31ab78e019
> slug:
> 	cs: bezpecna-aplikace
> 	en: safe-application
> 
> publicationDate: '2019-10-01 14:19:04'
> mainCategoryId: '3666a8a6-f2a3-405d-8263-bd53c4301fb3'
> sourceContentHash: '74e52382b995b303550d8e4b783bbb84'

If you are serious about developing web applications and the site will later be available on the Internet, it is very important to address security.

Realistically, the following threats await developers:

- **The application has an internal error**, for example, because the programmer made a mistake that he or she did not notice when writing the code, or it only manifests itself "sometimes"
- **The server has a misconfiguration** or the environment **has changed** because the server admin changed the server behavior and the sites were not adapted for it. Alternatively, we are deploying to a new machine and don't know the exact configuration.
- Someone is trying to attack**, either an external attacker or a former employee from within the system.

All situations are extremely unpleasant and need immediate action. Designing an application so that it never fails is not technically possible.

During development, it is very important to test the code once it has been written, and if a bug occurs on the server, it is important to inform the developer immediately with an accurate description of the problem.

To detect bugs and monitor server health, I recommend the <a href="https://tracy.nette.org/">Tracy</a> tool, which logs all fatal errors, unhandled exceptions, and more to files directly on the server disk. Additionally, you can set up an email where notifications will be sent.

Interested in a security consultation for your application?

Contact me.
