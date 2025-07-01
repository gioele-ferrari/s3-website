
# S3 Static Website

In questo progetto ho utilizzato **Amazon S3 (Simple Storage Service)**, un servizio di archiviazione oggetti offerto da **AWS (Amazon Web Services)**.  
S3 consente di **salvare, archiviare e recuperare** qualsiasi quantità di dati, da qualsiasi parte del mondo, in qualsiasi momento.

---

## Obiettivo del progetto

L'obiettivo di questo progetto è comprendere come utilizzare **Amazon S3** per ospitare un **sito web statico**, e configurarne correttamente l'accesso pubblico.

---

## Fasi di sviluppo

### 1. Creazione del bucket

Ho creato un bucket S3 con le seguenti impostazioni:

- **Nome**: `s3-website-gioele` (deve essere univoco a livello globale)
- **ACL (Access Control List)**: abilitata
- **Blocca tutti gli accessi pubblici**: disabilitato  
- Le restanti opzioni sono state lasciate con i valori predefiniti.

---

### 2. Creazione del sito web statico

Ho realizzato un semplice sito web statico costituito dal file `index.html`, che si trova all'interno della cartella del progetto.

[Visualizza file index.html](index.html)

---

### 3. Abilitazione dell'hosting statico

Nella sezione **"Proprietà"** del bucket ho abilitato l'opzione:

> **Modifica l'hosting di siti Web statici** → **Attiva**

Ho impostato come documento di indice: `index.html`.

---

### 4. Primo test del link pubblico

Dopo aver abilitato l'hosting, ho provato ad accedere al sito dal seguente URL:

[http://s3-website-gioele.s3-website.eu-central-1.amazonaws.com](http://s3-website-gioele.s3-website.eu-central-1.amazonaws.com)

Tuttavia, il browser restituiva un **errore 403 – Accesso negato**.

> Questo errore si verifica perché, per impostazione predefinita, **S3 protegge i file** caricati nei bucket, anche se è stato attivato l'hosting statico.

---

### 5. Pubblicazione del file `index.html`

Per rendere visibile il sito:

- Sono tornato nella sezione **"Oggetti"** del bucket.
- Ho selezionato `index.html`.
- Dal menu **"Operazioni"** ho scelto **"Rendi pubblico utilizzando ACL"**.

Il file era ora accessibile pubblicamente.

---

### 6. Verifica finale

A questo punto, il sito risultava correttamente visualizzabile al link:

🌐 [http://s3-website-gioele.s3-website.eu-central-1.amazonaws.com](http://s3-website-gioele.s3-website.eu-central-1.amazonaws.com)

---

### 7. Protezione del file da cancellazioni

Per evitare la cancellazione accidentale del file `index.html`, ho creato la seguente **bucket policy**:

```json
{
  "Version": "2012-10-17",
  "Id": "MyBucketPolicy",
  "Statement": [
    {
      "Sid": "BucketPutDelete",
      "Effect": "Deny",
      "Principal": "*",
      "Action": "s3:DeleteObject",
      "Resource": "arn:aws:s3:::s3-website-gioele/index.html"
    }
  ]
}
```

> Questa policy nega a **chiunque**, incluso l'owner del bucket, il permesso di eliminare `index.html`.  
> Neanche un utente IAM con privilegi elevati può cancellare il file, finché la policy è attiva.

---

### 8. Eliminazione del progetto

Infine, per evitare il superamento dei limiti del **piano gratuito AWS Free Tier**, ho:

- **Rimosso la bucket policy**
- **Eliminato l’oggetto**
- **Cancellato il bucket**

---

## Conclusioni

Questo esperimento mi ha permesso di capire come funziona il **deployment di siti statici tramite Amazon S3**, come gestire l’accesso pubblico e applicare policy di protezione per evitare modifiche indesiderate.
