<p align="center">
  <img src="https://raw.githubusercontent.com/YPSI-SAS/oneneurone/main/media/logo.png" alt="OneNeurone" width="128" />
</p>

<h1 align="center">OneNeurone</h1>

<p align="center">
  <strong>Plugin IA pour <a href="https://github.com/YPSI-SAS/ZeroNeurone">ZeroNeurone</a></strong><br>
  <em>Le Neurone propose, l'analyste dispose.</em>
</p>

<p align="center">
  <a href="https://doc.oneneurone.com">Documentation</a> &middot;
  <a href="#installation">Installation</a> &middot;
  <a href="#fonctionnalites">Fonctionnalites</a> &middot;
  <a href="CHANGELOG.md">Changelog</a>
</p>

---

OneNeurone ajoute une couche IA optionnelle a [ZeroNeurone](https://github.com/YPSI-SAS/ZeroNeurone), la plateforme d'investigation en sources ouvertes. Il assiste les analystes dans l'extraction d'entites, l'analyse de graphe, la generation de rapports et le dialogue contextuel — sans jamais modifier le graphe automatiquement.

## Fonctionnalites

| Fonctionnalite | Description |
|---|---|
| **Dialogue contextuel** | Interrogez votre graphe en langage naturel. Le Neurone repond en s'appuyant uniquement sur les donnees de l'enquete. |
| **Extraction d'entites** | Detection automatique de personnes, organisations, lieux, vehicules, coordonnees... a partir de textes et documents. Validation manuelle obligatoire. |
| **Generation de rapports** | Brouillons structures selon 5 registres : judiciaire, renseignement, corporate, journalistique, CERT/SOC. |
| **Impulsions** | Suggestions algorithmiques et IA : noeuds isoles, patterns, anomalies temporelles, ponts entre clusters. |
| **Analyse croisee** | Detection de liens entre enquetes distinctes : entites similaires, modes operatoires, schemas temporels. |

## Compatibilite LLM

OneNeurone fonctionne avec des modeles locaux ou cloud :

| Fournisseur | Type | Remarque |
|---|---|---|
| [Ollama](https://ollama.ai) | Local | Recommande — 100% hors-ligne |
| [LM Studio](https://lmstudio.ai) | Local | 100% hors-ligne |
| Anthropic (Claude) | Cloud | Pseudonymisation automatique |
| OpenAI | Cloud | Pseudonymisation automatique |
| Endpoint personnalise | Local/Cloud | API compatible OpenAI |

> Les fournisseurs cloud beneficient d'une **pseudonymisation automatique** des donnees sensibles avant envoi, avec chiffrement AES-GCM-256 des cles API en local.

## Installation

**Prerequis** : ZeroNeurone 2.15.4+

1. Telechargez `oneneurone.js` depuis ce depot (voir [Releases](https://github.com/YPSI-SAS/oneneurone/releases) ou le fichier a la racine)
2. Placez-le dans le dossier `plugins/` de votre installation ZeroNeurone
3. Creez ou mettez a jour `plugins/manifest.json` :

```json
{
  "plugins": [
    {
      "id": "oneneurone",
      "file": "oneneurone.js",
      "name": "OneNeurone",
      "description": "Le Neurone propose, l'analyste dispose"
    }
  ]
}
```

4. Rechargez la page

Le plugin est immediatement actif. Un bouton **Neurone** apparait dans l'en-tete de chaque investigation.

## Principes

- **L'analyste reste souverain** — Aucune action automatique. Chaque suggestion requiert une validation explicite.
- **Local-first** — Fonctionne integralement hors-ligne avec Ollama ou LM Studio. Aucune telemetrie.
- **Zero impact** — Si le plugin est retire, ZeroNeurone fonctionne identiquement. Les elements crees via le Neurone restent dans le graphe comme des elements normaux.
- **Confidentialite** — Pseudonymisation des donnees sensibles pour les fournisseurs cloud, chiffrement des secrets en local.

## Documentation

La documentation complete est disponible sur **[doc.oneneurone.com](https://doc.oneneurone.com)** (francais et anglais).

## Licence

Logiciel proprietaire — [YPSI SAS](https://ypsi.fr)

Une licence d'evaluation est incluse. Pour une licence commerciale, contactez [contact@ypsi.fr](mailto:contact@ypsi.fr).
