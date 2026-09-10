# Pipeline troubleshooting checklist

Ao receber uma falha, siga o caminho completo em vez de olhar apenas o último job:

1. O `include` resolveu o template/ref esperado?
2. `rules`/branch permitiram o job?
3. Build/test/security passaram?
4. A imagem/tag esperada foi produzida?
5. O runner possui `oc` e está autenticado?
6. O namespace correto foi selecionado?
7. O manifest foi aplicado?
8. Deployment criou ReplicaSet/Pod?
9. Readiness ficou Ready?
10. Service possui endpoints?
11. Route aponta para o Service/porta corretos?

Comandos úteis: `oc get pods`, `oc describe pod`, `oc logs`, `oc get events --sort-by=.lastTimestamp`, `oc get svc`, `oc get endpoints`, `oc get route`.
