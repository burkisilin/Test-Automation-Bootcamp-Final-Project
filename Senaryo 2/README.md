# Test Automation Bootcamp API Entegrasyon Testleri!
![image](https://user-images.githubusercontent.com/13181041/149951074-e9759bc2-5021-416a-a2a0-c346b098bca3.png)


Projede kullanılan endpointlere bu linkten ulaşabilirsiniz : https://bootcampapi.techcs.io/api/fe/v1/#/

# PROJE GEREKSİNİMLERİ

Projenin ayağa kalması için Postman ve aşağıdaki görselde paylaşılan Environmente ihtiyaç duyulmaktadır. İndirdiğiniz Collection'u Postman'ın içerisine import ettikten sonra belirtilen Environment'ı içe aktararak aktif olarak seçmeniz gerekmektedir.

![image](https://user-images.githubusercontent.com/13181041/149951413-c394c259-7a07-4403-af20-ed3f052d41ab.png)





# Sign Up Success
![image](https://user-images.githubusercontent.com/13181041/149616657-be020cfa-32cf-4159-9e7f-e2f796aa898b.png)

Pre-request Script üzerinde rastgele üretilen mail ve password değerleri Environment üzerine set edilir.

- Request Body is Valid
```javascript
pm.test("Request Body is Valid", () =>{
    
    pm.expect(typeof(email) == "string").to.be.true;
    pm.expect(typeof(password) == "string").to.be.true;
    pm.expect(validateEmail(email)).equal(true,"Email is not valid. "); // Check if email is valid.
});
```

Request Body'de bulunan;

email'in String cinsinden olup olmadığının kontrolü,
password'un String cinsinden olup olmadığının kontrolü,
validateEmail fonksiyonu ile email'in geçerli bir email olup olmadığının kontrolü sağlanmıştır.



- Signed Up Succesfully
```javascript
pm.test('Signed Up Successfully.', () =>{
pm.expect(typeof(jsonData.access_token) == "string").to.be.true
pm.expect(jsonData.access_token.length).to.be.equal(175)
});
```

Response Body'de bulunan;

access_token değerinin String cinsinden olup olmadığının kontrolü,
access_token değerinin 175 karakter içerip içermediğinin kontrolü sağlanmıştır.



- Status Code is 200
```javascript
pm.test('Status Code is 200', () =>{
    pm.expect(pm.response.code).equal(200,'Status received is ' + pm.response.code + '. User successfully signed-up -> Response code must be 200 due to Swagger API Documentation. '); // user successfully signed-up -> Response code must be 200 due to Swagger API Documentation. 
});
```
Response kodunun 200 olup olmadığı kontrol edilmiştir. Swagger üzerinde belirtildiği üzere 200 kodu beklenirken 201 kodunun dönmesi üzerine test fail olmaktadır.

# Sign In Success
![image](https://user-images.githubusercontent.com/13181041/149617747-8c976763-b27e-47d3-941b-ccf86adba97d.png)

Pre-request Script üzerinden sistemde kayıtlı bir mail ve password değeri Environment üzerine set edilir.


- Request Body Types are Valid
```javascript
pm.test("Request Body Types are Valid", () =>{
    pm.expect(typeof(email) == "string").to.be.true;
    pm.expect(typeof(password) == "string").to.be.true;
});
```
Request Body'de bulunan;

email'in String cinsinden olup olmadığının kontrolü,
password'un String cinsinden olup olmadığının kontrolü sağlanmıştır.


- Response Body Types are Valid
```javascript
pm.test("Response Body Types are Valid", () =>{
    pm.expect(typeof(jsonData.access_token) == "string").to.be.true
});
```
Response Body'de bulunan;

access_token değerinin String cinsinden olup olmadığının kontrolü sağlanmıştır.




- Signed In Successfully
```javascript
    pm.expect(pm.response.text()).to.include("access_token")
    pm.expect(jsonData.access_token.length).to.be.equal(175)
});
```

Response'de  "access_token" değerinin bulunup bulunmadığı ve access_token'in 175 karakterden oluşup oluşmadığı kontrol edilmiştir.

- Status Code is 200
```javascript
pm.test('Status Code is 200', () =>{
    pm.expect(pm.response.code).equal(200,'Status received is ' + pm.response.code + '. User successfully signed-up -> Response code must be 200 due to Swagger API Documentation. '); // user successfully signed-up -> Response code must be 200 due to Swagger API Documentation. 
});
```

Response kodunun 200 olup olmadığı kontrol edilmiştir. Swagger üzerinde belirtildiği üzere 200 kodu beklenirken 201 kodunun dönmesi üzerine test fail olmaktadır.

# Sign In Fail - Wrong Credentials
![image](https://user-images.githubusercontent.com/13181041/149618932-8f0ea5ad-108d-4a6b-b3a5-3b0dc7cc1622.png)

Giriş bilgileri body üzerinden yanlış set edilmiştir. 
```json
{
  "email": "wrongMail",
  "password": "wrongPassword"
}
```

- Request Body Types are Valid
```javascript
pm.test("Request Body Types are Valid", () =>{
    pm.expect(typeof(email) == "string").to.be.true;
    pm.expect(typeof(password) == "string").to.be.true;
});
```
Request Body'de bulunan;

email'in String cinsinden olup olmadığının kontrolü, 
password'un String cinsinden olup olmadığının kontrolü sağlanmıştır.

- Response Body Types are Valid
```javascript
pm.test("Response Body Types are Valid", () =>{
    pm.expect(typeof(jsonData.statusCode) == "number").to.be.true
    pm.expect(typeof(jsonData.message) == "string").to.be.true
});
```

- Unsuccessful Sign In
```javascript
pm.test('Unsuccessful Sign In', () =>{
    pm.expect(pm.response.text()).to.include("Unauthorized")
    pm.expect(pm.response.text()).to.not.include("access_token")
});
```
Response Body'de;
Unauthorized kısmının bulunduğu,
acces_token'in bulunmadığı kontrol edilmiştir.

- Status Code is 401
```javascript
pm.test('Status Code is 401', () =>{
    pm.expect(pm.response.code).equal(401,'Status received is ' + pm.response.code); // Response code must be 401 due to Swagger API Documentation. 
});
```
Response kodunun 401 olup olmadığı kontrol edilmiştir. 

# Sign Up Fail - Invalid Fields
![image](https://user-images.githubusercontent.com/13181041/149619152-ab6745c2-fb9b-423e-9b05-fff096029eea.png)

```javascript
function randomStringGenerator(length){
    var charset = 'abcdefghkmnpqrstuvwxyz23456789';
    return new Array(length)
      .fill(null)
      .map(()=> charset.charAt(Math.floor(Math.random() * charset.length)))
      .join('');

}

function setRequestBody(emailField,passwordField)
{
    switch (emailField)
    {
        case "validEmail":
            email = randomStringGenerator(3) + "@" +randomStringGenerator(5)+ ".com"
            break
        case "invalidEmail":
            email = randomStringGenerator(6) + ".com"
            break
        case "emptyEmail":
            email = ""
            break
    }

        switch (passwordField)
    {
        case "validPassword":
            
            password = randomStringGenerator(8)
            break
        case "invalidPassword":
            moreChars = Math.random() < 0.5 // Determine with %50 probability, if password is going to be invalid because of having more than 20 or below 8 characters. If password is going to be 20+ characters.
            if (moreChars == true){ // Determines the invalid password lenght to be below 8 or more then 20 chars.
                passwordLength = Math.floor(Math.random() * 11) + 21; // Set length from 21 to 31
            }
            else{
                passwordLength = Math.floor(Math.random() * 7) + 1; // Set length from 1 to 7
            }
            password = randomStringGenerator(passwordLength)
            break
        case "emptyPassword":
            password = ""
            break
    }

    pm.environment.set("email", email)
    pm.environment.set("password", password)
}

// Code Down Below Generates All Invalid 4 Scenarios

EmailConditions = ["invalidEmail", "emptyEmail"];
//PasswordConditions = ["invalidPassword", "emptyPassword"];

var counter = parseInt(pm.environment.get("counter"))

currentEmailCondition = EmailConditions[counter]


let PasswordConditions = pm.collectionVariables.get("PasswordConditions"); 
if(!PasswordConditions || PasswordConditions.length == 0) {
    PasswordConditions = ["invalidPassword", "emptyPassword"];
}

let currentPasswordCondition = PasswordConditions.shift();
pm.collectionVariables.set("PasswordCondition", currentPasswordCondition)
pm.collectionVariables.set("PasswordConditions", PasswordConditions)

setRequestBody(currentEmailCondition, currentPasswordCondition);
pm.environment.set("invalidFields", currentEmailCondition + ", "+ currentPasswordCondition);

```
Geçersiz alanların ayarlanması için Postmanda bulunan Pre-request Script kısmından yararlanılmıştır. 
Yukarıda bulunan kodlar ile gönderilecek body'de bulunan email ve password otomatize şekilde geçersiz veya boş olarak tanımlanmaktadır.
![image](https://user-images.githubusercontent.com/13181041/149619373-a91de117-01d1-48a8-8a70-e8ea6075d58d.png)
Requestin yapılan 3. testi olan "Request Body Invalid Field Errors" testinde rastgele oluşturulup request gönderilen parametrelerin invalid/empty olup olmadığı görülebilmektedir. Tüm geçersiz senaryoların tek bir request üzerinde oluşturulması, collection koşumu sırasında mümkün olan TÜM geçersiz parametre testlerinin tek bir request ile yapılmasını sağlamaktadır.


![image](https://user-images.githubusercontent.com/13181041/149952051-5eb1b2bc-24f5-4f1d-9c27-4af0cbbbb1ac.png)






- Request Body Types are Valid
```javascript
pm.test("Request Body Types are Valid", () =>{
    pm.expect(typeof(email) == "string").to.be.true;
    pm.expect(typeof(password) == "string").to.be.true;
});
```
Request Body'de bulunan;
email'in String cinsinden, password'un String cinsinden olup olmadığının kontrolü sağlanmıştır.

- Response Body Types are Valid
```javascript
pm.test("Response Body Types are Valid", () =>{
    pm.expect(typeof(jsonData.statusCode) == "number").to.be.true;
    pm.expect(typeof(jsonData.message) == "object").to.be.true;
    pm.expect(typeof(jsonData.error) == "string").to.be.true;
});
```
Response Body'de bulunan;
statusCode'nin number cinsinden, 
message'nin string cinsinden,
error'un string cinsinden olup olmadığınının kontrolü sağlanmıştır.

- Request Body Invalid Field Errors Case
```javascript
pm.test("Request Body Invalid Field Errors Case: "+ invalidFieldsCase, () =>{
    if (validateEmail(email) == false){
        pm.expect(pm.response.text()).to.include("email must be an email")
    }

    if (email.length == 0){
        pm.expect(pm.response.text()).to.include("mail should not be empty")
    }

    if (validatePassword == false){
        pm.expect(pm.response.text()).to.include("password must be shorter than or equal to 20 characters")
    }

    if (password.length == 0){
        pm.expect(pm.response.text()).to.include("password should not be empty")
    }
});
```
Gönderilen geçersiz alanlara göre dönmesi beklenen hata mesajlarının testi yapılmıştır.

- Status Code is 400
```javascript
pm.test('Status Code is 400', () =>{
    pm.expect(pm.response.code).equal(400,'Status received is ' + pm.response.code); // Respopnse code must be 400 due to Swagger API Documentation.
});
```
Response kodunun 400 olup olmadığı kontrol edilmiştir.



```javascript
var counter = parseInt(pm.environment.get("counter"))
if (password.length == 0) {
    counter += 1
    pm.environment.set("counter", counter)
}
if (counter < 2){
    postman.setNextRequest("Sign Up Fail - Invalid Fields");
} 
else {
    
        pm.environment.set("counter", 0)
}
```

Yukarıda belirtilen kod bloğu sayesinde tüm "Invalid Field" senaryoları tek bir request üzerinden, aynı requesti farklı parametreler ile çağırarak yönetilmiştir.

# Sign Up Fail - User Already Signed-Up
![image](https://user-images.githubusercontent.com/13181041/149620007-53f15cb1-8fc7-42a1-9dfc-46995a70aae0.png)


Daha önceden sisteme kayıtlı email adresi Pre-request Script kısmında aşağıdaki gibi ayarlanmıştır.
```javascript
pm.environment.set("email", "deneme@db.com")
pm.environment.set("password", "12345678")
```

- Request Body Valid
```javascript
pm.test("Request Body Valid", () =>{
    pm.expect(typeof(email) == "string").to.be.true;
    pm.expect(typeof(password) == "string").to.be.true;
    pm.expect(validateEmail(email) && validatePassword(password)).to.be.true;
});
```
Request Body'de bulunan; email'in String cinsinden, password'un String cinsinden ve email-password değerlerinin geçerli olup olmadığının kontrolü sağlanmıştır.

- Response Body Types are Valid
```javascript
pm.test("Response Body Types are Valid", () =>{
    pm.expect(typeof(jsonData.statusCode) == "number").to.be.true;
    pm.expect(typeof(jsonData.message) == "string").to.be.true;
    pm.expect(typeof(jsonData.error) == "string").to.be.true;
});
```
Response Body'de bulunan; statusCode'nin number cinsinden, message'nin string cinsinden, error'un string cinsinden olup olmadığınının kontrolü sağlanmıştır.

- Unsuccessful Sign Up
```javascript
pm.test('Unsuccessful Sign Up', () =>{
    pm.expect(pm.response.text()).to.include("User already exist!")
    pm.expect(pm.response.text()).to.include("Conflict")
    pm.expect(pm.response.text()).to.not.include("access_token")
});
```
Dönen Responsede;
"User already exist!", "Conflict" değerlerinin bulunduğu,
"access_token" değerinin bulunmadığı test edilmiştir

- Status Code is 409
```javascript
pm.test('Status Code is 409', () =>{
    pm.expect(pm.response.code).equal(409,'Status received is ' + pm.response.code); // Respopnse code must be 409 due to Swagger API Documentation.
});
```
Response kodunun 409 olup olmadığı kontrol edilmiştir.
