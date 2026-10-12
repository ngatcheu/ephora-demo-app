# ephora-demo-app

Application de démonstration pour [ephora-operator](https://github.com/ngatcheu/ephora-operator) : chaque Pull Request obtient son propre environnement de preview, qui affiche le numéro de la PR et sa révision.

- `charts/app/` : chart Helm (nginx non-root, compatible avec le niveau Pod Security `restricted`)
- `preview-template.yaml` : le `PreviewEnvironment` d'une PR
- `.github/workflows/preview.yml` : création, mise à jour et suppression de l'environnement à chaque événement de PR

## Tester le chart en local

```bash
helm lint charts/app
helm template demo charts/app --set preview.prNumber=42 --set preview.revision=abc123
```

## Créer un environnement à la main (comme la CI)

```bash
export PR_NUMBER=42 REVISION=$(git rev-parse HEAD) PREVIEW_MGMT_NAMESPACE=ephora-previews
envsubst < preview-template.yaml | kubectl apply -f -
kubectl -n ephora-previews get penv -w
```
