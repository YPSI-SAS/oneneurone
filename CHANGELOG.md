# Changelog

## 1.1.1 — 2026-02-23

## 1.1.0 — 2026-02-22

### Extraction NER
- **GLiNER** : pre-detection d'entites en local via ONNX (~150 MB) pour ameliorer la precision LLM
- Separation des indices GLiNER en candidats entites vs candidats proprietes (lieux, dates, montants)
- Classification automatique du type de document (8 categories) pour adapter le prompt NER
- Deduplication automatique des entites extraites (par nom, fusion des proprietes)
- Parsing tolerant : accepte les variantes de format LLM (`id`/`name`, `score`/`confidence`, `source_id`/`fromEntity`)
- Evenements sans entite associee sont conserves (placeholder)
- Bouton **Restaurer** sur les entites rejetees (retour en arriere)
- Creation des relations et evenements lors de l'acceptation individuelle (pas seulement en bulk)

### Ollama
- Migration vers l'API native `/api/chat` (au lieu de `/v1/chat/completions`)
- Support `num_ctx` pour configurer la fenetre de contexte (evite la troncature a 4096)
- `format: 'json'` natif pour le mode JSON
- `keep_alive: '5m'` pour garder le modele en memoire

### Configuration
- **Slider budget tokens** : 7 paliers fixes (2K, 4K, 8K, 16K, 32K, 64K, 128K) pour les providers locaux
- Section **Avance** (accordeon) regroupant format contexte, budget tokens et GLiNER
- Le budget tokens est applique directement (plus de scaling proportionnel)

### Prompts
- Prompts NER specialises par type de document (8 variantes)
- Regles de discrimination ameliorees pour la classification
- Instructions explicites sur les 3 tableaux JSON (entities, relationships, events)

### Corrections
- Fix "Similaire a «»" quand l'element est supprime avant acceptation
- Fix duplicates warning sur label vide

## 1.0.0 — 2026-02-22

- Release initiale

## 0.10.0-beta — 2026-02-22

- Initial release