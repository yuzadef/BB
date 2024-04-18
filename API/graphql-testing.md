- [ ] navigate through application normally | use network module to identify graphql endpoint
- [ ] click on all links, inputs where you can -> get all api endpoints/features; fire up BS/POSTMAN to capture requests
- [ ] bruteforce for graphql endpoint, look for graphql ide | test for versioning: /api/v2/ -> /api/v1/
```
• /graphql
• /v1/graphql
• /graphiql
```
- [ ] using introspection if possible to get api schemas | interact with graphql ide if enabled
- [ ] interact with the api in intended and unintended ways | understanding the application
- [ ] fuzzing wide & fuzzing deep the inputs, headers, authentication value, request body
```	
• database queries | system commands
• a string of letters when number is expected | longer strings when shorter strings is expected
• various special characters | emoticons | characters from unexpected language (e.g: chinese)
• request methods
• improper assets management (v1, v2, internal, dev, non-prod, mobile, uat, old)
• fuzz for escape characters | null bytes, traversal characters, encoded characters
• fuzz for directory traversal
```
