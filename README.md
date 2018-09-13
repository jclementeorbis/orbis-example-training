# Paso 3

## Descargar el proyecto
```sh
$ git clone git@bitbucket.org:orbisunt/orbis-example-training.git
```

## Cambiar el repositorio remoto origin por uno de igual nombre en tu cuenta github. (Crear en github el repositorio y enlazarlo)
```sh
$ git remote rm origin
$ git remote add origin git@github.com:jclementeorbis/orbis-example-training.git
```

## Subir los cambios a github ¿Qué mensaje aparece?
```sh
$ git push origin master
```
```sh
git push origin master
Counting objects: 20, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (12/12), done.
Writing objects: 100% (20/20), 147.22 MiB | 3.20 MiB/s, done.
Total 20 (delta 6), reused 20 (delta 6)
remote: Resolving deltas: 100% (6/6), done.
remote: error: GH001: Large files detected. You may want to try Git Large File Storage - https://git-lfs.github.com.
remote: error: Trace: 3612cecaccdd2870dbd5da606dfd15ee
remote: error: See http://git.io/iEPt8g for more information.
remote: error: File sc.16.tar.gz is 146.81 MB; this exceeds GitHub's file size limit of 100.00 MB
To github.com:jclementeorbis/orbis-example-training.git
 ! [remote rejected] master -> master (pre-receive hook declined)
error: failed to push some refs to 'git@github.com:jclementeorbis/orbis-example-training.git'
```

## Ahora el objetivo de esta parte es identificar el archivo más pesado que se encuentra en el repositorio y eliminarlo.
```sh
$ git verify-pack -v .git/objects/pack/pack-e121f70e46f7ec3df6d405dfb4895762265c6af7.idx | sort -k 3 -n | tail -3
7044640436e9b16f956bbdc8d5329d10c8f07d62 blob   766 411 470939
2ce5eca830c1f68c365b5323677e7c37212eccf6 blob   479328 469875 1064
cfc3322ec593c88d0e4d68c312de3583ca741041 blob   153942735 153904153 471677
```

```sh
$ git rev-list --objects --all | grep cfc3322ec593c88d0e4d68c312de3583ca741041
cfc3322ec593c88d0e4d68c312de3583ca741041 sc.16.tar.gz
```

```sh
$ git filter-branch --index-filter 'git rm --cached --ignore-unmatch sc.16.tar.gz' HEAD
Rewrite f3a6dc74d8fd32278c93a5e9d4bbaec4d43c04c5 (3/7) (1 seconds passed, remaining 1 predicted)    rm 'sc.16.tar.gz'
Rewrite d270f6d507e5a9594183c7a64f112d7e798508fc (3/7) (1 seconds passed, remaining 1 predicted)    rm 'sc.16.tar.gz'
Rewrite 3bd289d96f8784ba7a3fd793364df80ebaaec1f3 (7/7) (2 seconds passed, remaining 0 predicted)
Ref 'refs/heads/master' was rewritten
```

## Subir los cambios
```sh
$ git push origin master
```