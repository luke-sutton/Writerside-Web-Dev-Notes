# Create a Standard API Response

You can create an API response object to return a standardised result format every time. For example in the repose object
rather than just return the data, we may wish to return whether it was successful, the status code and any error messages
etc. so the response body would look something like this -

```json
{
  "isSuccess": true,
  "result": [
    {
      "id": 1,
      "name": "10OFF",
      "percent": 10,
      "isActive": true,
      "created": null,
      "lastUpdated": null
    },
    {
      "id": 2,
      "name": " 20OFF",
      "percent": 20,
      "isActive": true,
      "created": null,
      "lastUpdated": null
    }
  ],
  "statusCode": 200,
  "errorMessages": []
}
```

To do this first create a new model called APIResponse and add the required properties -

```C#
using System.Net;

namespace MagicVilla_CouponAPI.Models;

public class APIResponse
{
    public APIResponse()
    {
        ErrorMessages = new List<string>();
    }
    
    public bool isSuccess { get; set; }
    public Object Result { get; set; }
    public HttpStatusCode StatusCode { get; set; }
    public List<string> ErrorMessages { get; set; }
}
```

Then in your endpoints, build up a new APIResponse object and return the results -

```C#
APIResponse response = new();
response.Result = await _couponRepo.GetAsync(id);
response.isSuccess = true;
response.StatusCode = HttpStatusCode.OK;
return Results.Ok(response);
```