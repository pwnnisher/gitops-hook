# Repositorio del gancho (lo empuja el organizador, una sola vez)

El exploit de `web/gitops` necesita un repositorio **público** de GitHub cuyo
`hooks.git/post-checkout` se ejecute en el servidor del reto. Este directorio es
exactamente lo que hay que empujar: el solver no crea ni modifica nada en GitHub.

Se empuja una vez y sirve para todos los eventos. No lleva credenciales.

```bash
cd web/gitops/writeup/hook-repo
git init -b main
git add -A
git commit -m "gitops: post-checkout hook that prints /flag.txt"
git remote add origin git@github.com:<OWNER>/<REPO>.git   # repo PUBLICO ya creado
git push -u origin main
```

Dos requisitos, y los dos se rompen en silencio si faltan:

1. El repositorio tiene que ser **público**: el reto clona sin credenciales.
2. `hooks.git/post-checkout` tiene que conservar el **bit de ejecución**. Git lo
   versiona, y este fichero ya lo trae; si lo recreas a mano, `chmod +x` y
   comprueba con `git ls-files -s hooks.git/post-checkout` que sale en modo
   `100755` y no `100644`.

Después:

```bash
python3 ../solve.py https://llaitun-lb.my-ctf.com:9422 --repo <OWNER>/<REPO>
```
