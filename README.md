# lnbits-webhook-example
Webhook example code for LNbits

Various endpoints in the LNbits API support using webhooks in form of adding a webhook URL in the webhook field. This is an example of using PHP and the create or pay invoice endpoint in the LNbits API.

Example of adding the webhook URL using cURL (this could be done on the dashboard as well):

    ```
    curl -X 'POST'
    https://your-lnbits-endpoint.com/api/v1/payments
    -H "X-Api-Key: <INVOICE KEY>"
    -H "Content-type: application/json"
    -d '{"out": false, "amount": 1, "memo": "test webhook", "webhook": "https://example.com/webhook.php"}'
    ```

The value of the webhook field refers to where you have the code that will process the data from the webhook. This example assumes that the code processing the data is found in https://example.com/webhook.php


Example code in webhook.php to process the webhook data:

    ```
    <?php
    
    if ($_SERVER['REQUEST_METHOD'] == 'POST') {

    // Webhook json content
    $json = file_get_contents('php://input'); 

    // Decode the json content
    $object = json_decode($json);

    // Check if the json is valid
    if (json_last_error() !== JSON_ERROR_NONE) {
        die(header('HTTP/1.0 415 Unsupported Media Type'));
    }

    // Log content to a file to inspect it to see the values of the object
    file_put_contents('object.txt', print_r($object, true));
    
    //In this example, I'm storing the checking_id into a variable.
    $checking_id = $object->checking_id;
    
    
    /**
    ** From here, you write the your code to process the data the way you want
    **
    **
    **/
    
    }
    
    ?>
    ```

