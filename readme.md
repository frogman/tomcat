```mermaid

sequenceDiagram
    autonumber
    actor user as Benutzer<br/>Inhaber
    participant wallet as RaiWallet 
    participant psp as Bank<br/>Aussteller und Prüfer

    Note over psp: Zahlungsinitiierung
    rect rgb(100, 150, 100)
    note over wallet, psp: Protokoll für überprüfbare Präsentationen
    psp ->> wallet: Autorisierungsanfrage (Transaktionsdaten)
    activate wallet
    wallet ->> user: Genehmigungsdialog anzeigen
    user ->> user: Überprüfung der Transaktion
    user ->> wallet: Benutzer genehmigt Präsentation
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
