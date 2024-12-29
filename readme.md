```mermaid

sequenceDiagram
    autonumber
    actor user as Benutzer<br/>Inhaber
    participant wallet as Wallet 
    participant psp as ASPSP<br/>aka Issuer & Verifier

    Note over psp: Out-of-band payment initiation
    rect rgb(100, 150, 100)
    note over wallet, psp: OpenID4VP
    psp ->> wallet: Authorization Request(transaction_data)
    activate wallet
    wallet ->> user: SHOW approval dialoge
    user ->> user: review transaction
    user ->> wallet: approves presentation
    wallet -->> psp: POST Authorization Response(P2Pay)
    psp -->> wallet: RESP OK, redirect_uri
    end
    wallet -->> user: SHOW presentation status
    deactivate wallet
    wallet ->> psp: GET PSP metadata
    psp -->> wallet: RESP PSP metadata, payment_status_uri
    psp ->> psp: verify P2Pay and execute transaction
    loop
    wallet ->> psp: GET payment-status(a2pay-id, payment-id)

    psp -->> wallet: RESP payment status 
    end
    wallet -->> user: SHOW payment status
    wallet ->> psp: GET follow redirect_uri

```
