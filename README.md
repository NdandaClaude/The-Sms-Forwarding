# The-Sms-Forwarding

Application de Récupération et d'Envoi de SMS
Cette application mobile permet de récupérer les SMS reçus sur le téléphone où elle est installée, puis de les envoyer à une API via une requête POST. Développée pour répondre à des besoins comme la collecte de votes publics ou les enquêtes SMS, cette application est polyvalente et dépend de son contexte d'utilisation.

Fonctionnalités

Récupération automatique des SMS entrants : l'application écoute les messages entrants et extrait le numéro de l'expéditeur (sender) ainsi que le contenu du message.
Envoi à une API REST : les données (numéro et message) sont transmises à une API via une requête POST, permettant leur stockage ou leur traitement dans un serveur distant.

Exemples de Code

Code de l'Application Mobile (extrait)
L'application envoie chaque SMS reçu au serveur avec la méthode suivante :

```typescript
async sendToApi(sender: string, message: string, date: Date) {
    try {
      let result = await fetch(this.apiUrl , {
        method: "POST",
        body: JSON.stringify({ sender: sender, message: message })
      }).then(rep => rep.json());

      if (result.status === 'success') {
        this.receivedMessages.push({ sender, message, date });
      } else {
        this.showError("Erreur d'enregistrement du message");
      }
    } catch (error) {
      this.showError(`Erreur: ${error}. URL de l'API: ${this.apiUrl}. Expéditeur: ${sender}. Message: ${message}`);
    }

    this.change.detectChanges();
}

```
### 3. Exemple de JSON

Pour un bloc de données JSON :

```json
```markdown
{
    "sender": "681297063",
    "message": "MUT12"
}
```

