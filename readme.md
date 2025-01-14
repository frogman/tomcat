```mermaid

sequenceDiagram
    autonumber
    actor user as Benutzer<br/>Inhaber
    participant wallet as RaiWallet 
    participant psp as Bank<br/>Aussteller und Prüfer

    Note over psp: Zahlungsinitiierung
    rect rgb(143, 153, 222)
    note over wallet, psp: Protokoll für überprüfbare Präsentationen
    psp ->> wallet: Autorisierungsanfrage (Transaktionsdaten)
    activate wallet
    wallet ->> user: Genehmigungsdialog anzeigen
    user ->> user: Überprüfung der Transaktion
    user ->> wallet: Benutzer genehmigt Präsentation
    wallet -->> psp: POST-Autorisierungsantwort(P2P-Zahlung)
    psp -->> wallet: Antwort OK, redirect_uri Web_Adresse
    end
    wallet -->> user: Präsentationsstatus anzeigen
    deactivate wallet
    wallet ->> psp: Abruf der Metadaten des Zahlungsdienstleisters
    psp -->> wallet: Antwort Zahlungsdienstleister Metadaten, status_Web_Adresse
    psp ->> psp: Überprüfung des Zahlungsdienstleisters <br/> Durchführung der Transaktion
    loop
    wallet ->> psp: Zahlungsstatus abrufen(Zahlungsschema-id, Zahlungs-id)

    psp -->> wallet: Antwort Zahlungsstatus 
    end
    wallet -->> user: Zahlungsstatus anzeigen
    wallet ->> psp: Web Adresse für die Weiterleitung erhalten

```
