java-figo [![Build Status](https://travis-ci.org/figo-connect/java-figo.png)](https://travis-ci.org/figo-connect/java-figo)
===========

Java bindings for the figo connect API. Find more detailed API docss at  http://docs.figo.io/v3

Simply add to your pom.xml:

```xml
<dependency>
        <groupId>me.figo</groupId>
        <artifactId>sdk</artifactId>
        <version>3.1.5</version>
</dependency>
```

And just as easy to use:
```java
import java.io.IOException;

import me.figo.FigoException;
import me.figo.FigoSession;
import me.figo.models.Account;
import me.figo.models.Transaction;


public class FigoExample {

	public static void main(String[] args) throws FigoException, IOException {
		FigoSession session = new FigoSession("ASHWLIkouP2O6_bgA2wWReRhletgWKHYjLqDaqb0LFfamim9RjexTo22ujRIP_cjLiRiSyQXyt2kM1eXU2XLFZQ0Hro15HikJQT_eNeT_9XQ");

		// print out a list of accounts including its balance
		for (Account account : session.getAccounts()) {
			System.out.println(account.getName());
			System.out.println(session.getAccountBalance(account).getBalance());
		}

		// print out the list of all transactions on a specific account
		for (Transaction transaction : session.getTransactions(session.getAccount("A1.2"))) {
			System.out.println(transaction.getPurposeText());
		}
	}

}
```

To add a proxy server to a FigoSession or FigoConnection do this:

```java
// first create the FigoSession object
FigoSession session = new FigoSession("ASHWLIkouP2O6_bgA2wWReRhletgWKHYjLqDaqb0LFfamim9RjexTo22ujRIP_cjLiRiSyQXyt2kM1eXU2XLFZQ0Hro15HikJQT_eNeT_9XQ");

// create a Proxy and add it to the session
Proxy proxy = new Proxy(Proxy.Type.HTTP, new InetSocketAddress("10.0.0.1", 8080));
session.setProxy(proxy);

// now do your API calls
```

You can add valid SSL fingerprints by adding them to the `FIGO_API_FINGERPRINTS` environment variable. Fingerprints
need to be added in HEX format without column delimiters. A column delimiter is used to indicate the next element in
the list of fingerprints.


To disable the SSL certificate pinning (not recommended) do the following:
```java
// first create the FigoSession object
FigoSession session = new FigoSession("ASHWLIkouP2O6_bgA2wWReRhletgWKHYjLqDaqb0LFfamim9RjexTo22ujRIP_cjLiRiSyQXyt2kM1eXU2XLFZQ0Hro15HikJQT_eNeT_9XQ");

// create FakeTrustManager and add it to your FigoConnection or FigoSession
X509TrustManager trustManager = FakeTrustManager();
session.setTrustManager(trustManager);

// now do your API calls
```

To connect to the staging system of figo, you need to set the `FIGO_API_FINGERPRINTS` environment variable with the staging SHA256 fingerprint (`B752322B4BDCC974B165A79E8DF944E5A1622DD34327A7AAF5F7B7FAD9C31B0A`)


A more detailed documentation of the figo connect API can be found at http://docs.figo.io/v3/.

Demos
-----
In this repository you can also have a look at a simple console(src/console_demo) and web demo(src/web_demo). While the console demo
simply accesses the figo API, the web demo implements the full OAuth flow.
