# Lightcone Relayer

{% api-method method="post" host="https://lightcone.io" path="/v1.1" %}
{% api-method-summary %}
API Endpoint
{% endapi-method-summary %}
{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% hint style="info" %}
All API has the same URI
{% endhint %}

{% api-method method="" host="https://lightcone.io" path="/api/v1" %}
{% api-method-summary %}
GetServerTime
{% endapi-method-summary %}

{% api-method-description %}
This endpoint allows you to get free cakes.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-headers %}
{% api-method-parameter name="Authentication" type="string" required=true %}
Authentication token to track down who is emptying our stocks.
{% endapi-method-parameter %}
{% endapi-method-headers %}

{% api-method-body-parameters %}
{% api-method-parameter name="" type="object" required=true %}
```javascript
{
  "jsonrpc": "2.0",
  "method": "loopring_getServerTime",
  "params": [],
  "id": 1
}
```
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
Cake successfully retrieved.
{% endapi-method-response-example-description %}

```javascript
{
  "id":1,
  "jsonrpc": "2.0",
  "result": 1548410119809
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

