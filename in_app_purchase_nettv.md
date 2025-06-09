

# NETTV  In-app purchase implementation

  
Mobile in-app purchases (IAPs) allow users to purchase digital content or services within a mobile app. Implementing IAPs can be a great way to monetize your mobile app and generate revenue.

There are several types of in-app purchases (IAPs) that mobile app developers can offer to their users. You can find details of these types in following links:

**iOS**:  [https://developer.apple.com/in-app-purchase/](https://developer.apple.com/in-app-purchase/)

The specific types of IAPs that are appropriate for your app will depend on the app's content and business model. For NETTV, we have implemented a non-renewing type for in-app product subscription.
##

  
#### Basic flow :
1. **User Starts the Purchase**: User makes subscription for product subscriptions and taps the “Buy” button to make an in-app purchase.
    
2.  **Apple Displays Purchase Dialog**: Apple’s system steps in and shows a secure dialog box, asking the user to confirm the subscription purchase. This usually includes signing in with their Apple ID and approving the payment.
    
3.  **Purchase Outcome**: On Success, If the payment goes through, Apple generates a unique transaction ID and sends it to app. On failure,  If something goes wrong Apple shows an error message to the user, and the process stops here.
    
4.  **Server Validation**: App sends the transaction ID to server, which securely checks it with Apple’s App Store server to confirm the purchase is legit.

5. **Final Result**: On, Success If Apple’s server verifies the transaction, servers registers the purchase. App gets a “success” message, and gets access to content. On, Failure: If the validation fails (e.g., due to an invalid transaction ID or some issue), server sends an error message back to the app.

```mermaid 
sequenceDiagram
    participant User
    participant App
    participant AppleSystem as Apple System
    participant Server
    participant AppStoreServer as App Store Server

    User->>App: Taps "Buy" button for subscription
    App->>AppleSystem: Initiates in-app purchase
    AppleSystem->>User: Displays secure purchase dialog
    User->>AppleSystem: Confirms purchase (Apple ID & payment)
    
    alt Purchase Success
        AppleSystem->>App: Sends unique transaction ID
        App->>Server: Sends transaction ID for validation
        Server->>AppStoreServer: Validates transaction ID
        AppStoreServer->>Server: Confirms transaction validity
        
        alt Validation Success
            Server->>App: Sends "success" message
            Server->>App: Registers purchase
            App->>User: Grants access to content
        else Validation Failure
            Server->>App: Sends error message
            App->>User: Displays error message
        end
    else Purchase Failure
        AppleSystem->>User: Shows error message
    end
```
##
#### Restore purchase:
1. **User Restore purchase**: User intiate restore purchase which he/she has already bought in other devices or want to restore in same device due to app deletion.

2. **Restore Outcome**: App send "transaction id" and "transaction type" to validate to server.

3. **Final result**: On, Success, App gets "success message" and gets access to content. On Failure: server sends an error message back to app

```mermaid
sequenceDiagram
    participant User
    participant App
    participant Server

    User->>App: Initiates restore purchase
    App->>Server: Sends transaction ID and type
    Server-->>App: Validates transaction
    alt Success
        Server-->>App: Returns success message
        App->>User: Grants content access
    else Failure
        Server-->>App: Returns error message
        App->>User: Displays error
    end
```
##
### Refund Purchase Process

Refunds for in-app purchases cannot be initiated within the app. Users must request a refund externally through Apple's refund process, as detailed in [Apple's Refund Support](https://support.apple.com/en-us/118223). For developers, a specific code implementation is available to test refund functionality.

#### Developer Testing (Refund Implementation)
To simulate and verify refund behavior during testing, developers can use the following code:

```dart
IAPMethodCall().requestRefund($transactionId)
```


This method initiates a refund request for a specified transactionId, enabling developers to test refund workflows.
##
### Important Note
*As of StoreKit2 implementation,In-App Purchase Payment is only available on iOS devices running iOS 15.0 or later.*
##

### Payment Methods

The app offers three flexible payment methods, allowing users to select their preferred option. These methods can be customized by enabling or disabling them through the configuration "[Dynamic Content](https://iptv-admin.geniustv.dev.geniussystems.com.np/dynamic-content/edit/230)."

#### Available Payment Methods
- Wallet Payment
- E-Payment
- In app purchase
#


