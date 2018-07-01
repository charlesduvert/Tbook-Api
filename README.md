# T-book Web Service API

Our web service API provides users with fast and reliable access to your T-book data. You can use the T-book Web Service API to retrieve and manage T-book content.

## Getting Started

This section describes how to access our Web Service API.

### Prerequisites

First, you need to generate your API credentials : 

* Login to your T-book client account
  - [Test platform](https://test.t-book.fr)
  - [Live platform](https://t-book.fr)
* Go to the API section. If you can't find this section, please [contact us](mailto:alban.miconnet@digitaluniversity.education), we will have to grant you access. 
* On the API page of your account, you will find your Client ID, and be able to generate your Client Secret. **BEWARE : you will only be able to see your Client Secret this one time!** Store precautiously your credentials, they will be needed in any request to the API. You will however be able to regenerate your Client Secret if needed.

> Note : Your Client Secret is confidential. Avoid storing it in plain text. 

### Endpoints

All paths in the documentation are relative to the base API url. 
* **Test mode** : xxx
* **Live mode** : xxx

## Authentication

The access to the API is private. Therefore, you need to provide a JSON Web Token (JWT) as a mean of authentication. Tokens can be obtained by using the token endpoint with your API credentials. Replace your Client ID and Client Secret with your credentials.

**Request**
```
curl -X POST /token \
  -F grant_type= client_credential \
  -F client_id= Your_Client_Id \
  -F client_secret= Your_Client_Secret \
```

### Break down into end to end tests

Explain what these tests test and why

```
Give an example
```

### And coding style tests

Explain what these tests test and why

```
Give an example
```

## Deployment

Add additional notes about how to deploy this on a live system

## Built With

* [Dropwizard](http://www.dropwizard.io/1.0.2/docs/) - The web framework used
* [Maven](https://maven.apache.org/) - Dependency Management
* [ROME](https://rometools.github.io/rome/) - Used to generate RSS Feeds

## Contributing

Please read [CONTRIBUTING.md](https://gist.github.com/PurpleBooth/b24679402957c63ec426) for details on our code of conduct, and the process for submitting pull requests to us.

## Versioning

We use [SemVer](http://semver.org/) for versioning. For the versions available, see the [tags on this repository](https://github.com/your/project/tags). 

## Authors

* **Billie Thompson** - *Initial work* - [PurpleBooth](https://github.com/PurpleBooth)

See also the list of [contributors](https://github.com/your/project/contributors) who participated in this project.

## License

This project is licensed under the MIT License - see the [LICENSE.md](LICENSE.md) file for details

## Acknowledgments

* Hat tip to anyone whose code was used
* Inspiration
* etc
