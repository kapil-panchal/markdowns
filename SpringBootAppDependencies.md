# SpringBoot Application Dependencies

## 1. Application Dependencies
```java
ResponseEntity<String> response = restTemplate.getForEntity(apiUrl, String.class);
		HttpStatusCode statusCode = response.getStatusCode();
	    String responseBody = response.getBody();
	        
	    System.err.println("Status code: " + statusCode);
	    System.err.println("Response body: " + responseBody);
		
		return ResponseEntity.ok(Mono.just("Get All Data From Webclient"));
``` 