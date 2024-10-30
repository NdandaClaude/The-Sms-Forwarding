# The-Sms-Forwarding

Application de Récupération et d'Envoi de SMS
Cette application mobile permet de récupérer les SMS reçus sur le téléphone où elle est installée, puis de les envoyer à une API via une requête POST. Développée pour répondre à des besoins comme la collecte de votes publics ou les enquêtes SMS, cette application est polyvalente et dépend de son contexte d'utilisation.

## Fonctionnalités

Récupération automatique des SMS entrants : l'application écoute les messages entrants et extrait le numéro de l'expéditeur (sender) ainsi que le contenu du message.
Envoi à une API REST : les données (numéro et message) sont transmises à une API via une requête POST, permettant leur stockage ou leur traitement dans un serveur distant.

### Exemples de Code

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
{
    "sender": "681297063",
    "message": "MUT12"
}
```

### Exemple d'API en PHP

Le code suivant montre comment l'API côté serveur reçoit et enregistre les données transmises :

```PHP
<?php
header("Access-Control-Allow-Origin: *");
header("Access-Control-Allow-Methods: POST, OPTIONS");
header("Access-Control-Allow-Headers: Content-Type, Authorization");

include 'config.php';

if ($_SERVER['REQUEST_METHOD'] === 'POST') {
    $input = file_get_contents('php://input');
    $data = json_decode($input, true);

    file_put_contents('request_log.txt', "Requête reçue le " . date('Y-m-d H:i:s') . " :\n" . $input . "\n\n", FILE_APPEND);

    $sender = $data['sender'] ?? null;
    $message = $data['message'] ?? null;

    if ($sender && $message) {
        $sql = "INSERT INTO sms_messages (sender, message) VALUES (:sender, :message)";
        $stmt = $pdo->prepare($sql);
        $stmt->execute(['sender' => $sender, 'message' => $message]);

        echo json_encode(["status" => "success", "message" => "SMS enregistré"]);
    } else {
        echo json_encode(["status" => "error", "message" => "Données manquantes"]);
    }
} else {
    echo json_encode(["status" => "error", "message" => "Méthode non autorisée"]);
}
?>


```






## Installation

Clonez le projet : Téléchargez l'application et configurez l'API.
Installez l'APK : Installez le fichier APK sur votre téléphone.
Configurez l'URL de l'API : Spécifiez l'URL de l'API dans les paramètres de l'application pour que les messages soient envoyés au bon serveur.

## Utilisation

Votes publics : Récupération de votes envoyés par SMS.
Enquêtes et Sondages : Collecte des réponses d'enquête envoyées par SMS pour les analyser en temps réel.
Remarque : Cette application est un outil neutre, dont l’éthique et l’utilisation relèvent de la responsabilité de l'utilisateur.

## Sécurité et Confidentialité

L'application manipule des données sensibles (messages SMS et numéros). Son utilisation doit donc être conforme aux lois et aux règlements en vigueur.

## Avertissement

Cette application n'a pas pour but d’être utilisée à des fins malveillantes. Elle a été conçue à des fins d’expérimentation dans un cadre respectueux de la vie privée et des lois sur la collecte de données.

