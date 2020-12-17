## FORMACION GIT

-para editor simple de texto usar nano, para los merge desde bash :wq

## Genericos

#cd mkdir ls
#touch filename         	      			crea fichero
#start o explorer .         	            abre el explorador de windows de esa ruta
#rm fichero 	o 		rm -rf directorio	borrar

--

#git init 							Empezar un nuevo proyecto git
#git status							Ver el estado actual, si hay ficheros nuevos o modificados
#git log 							Ver el log de lo diferentes commits y branchs nuevos
#git diff 							Ver las diferencias / conflictos que pueda haber entre los diferentes ficheros.

	Si lo utilizamos sin el global, se usa para un determinado repositorio.
#git config --global user.name "Gino Angles"
#git config --global user.email pangles@nalandaglobal.com
#git config --list

#git ls-files -s  				LISTA EL STAGING AREA

#git add [filename]
#git rm --cached [file4.txt]

#git commit -m ["Nuestro primer coomit de dos ficheros txt"]

#git log --oneline
#git log --graph
#git log -2
#git log --grep="añadien" --oneline
#git log --no-merges --oneline

#git reset --soft o --hard, por defecto --mixed   					la idea es borrar un commit y cambiar el head al commit anterior hijo

#git revert 														se mueve a un commit anterior

## Acceder a Objects Git

#git cat-file -p [fd7524bf]			con el hexadecimal SHA1, los 5 o 6 primeros hex
#git cat-file -z [fd7524bf]			tamaño del fichero objecto
#git cat-file -t [fd7524bf]			tipo del object type (Blob, Tree, Commit, Tag)

#git hash-object [file3.txt] -w 		con -w guardamos ese object dentro de git

find .git/objects/ -type f 			busca los tipos ficheros guardados en git

## Crear tree (Para crear objectos en git, luego pasarlos al Staging y por ultimo traerlos a local)
	#Tree de ejemplo
	{

	100644 blob 1b3c2a41ee19149c2542d52a05bd271dbbf2f920	fichero1.txt
	100644 blob 5b7c83d8a0c68cef85a46a28d2fc1008be015a59	fichero2.txt
	}
#cat [temp_tree.txt] | git mktree 		Tabulador entre hex y filename
#git read-tree [42523]					leer el arbol para llevarlo al stagin area

## Staging area leer y traer a local

#git ls-files -s
#git checkout-index -a 				traernos desde el staging area a local

## Branchs
ls -la .git/refs/heads 				en heads se guardan las referencias a las ramas.
cat .git/HEAD 						para ver a donde apunta tu puntero en este momento (En que rama estamos actualmente).

#git branch -vv
#git branch [rama] [sha1] 			crear una rama a partir de un determinado commit sha1
#git branch -d [nombre rama]		para borrar la rama
#git checkout [rama]				cambiar a ramas
#git checkout [sha1]				cambiar a commit

#git checkout [master] -b [BR-1] 		creamos y cambiamos a la rama BR1 de una copia de Master
#git checkout -b [BR-1] 				creamos y cambiamos a la rama BR1 de una copia de la rama actual

#git branch -m [nombre actual] [nombre nuevo] 				renombrar el branch

## Merge Fast-Forward

#git merge [branch name]     							
	Si hay conflictos te lista un lista de ficheros, que tienes que resolver y mergear                 :wq    por bash
#git commit -a -m [mensaje del commit/merge] 														una vez resuelto los conflictos

## Clone de un repository

#git clone [https://github.com/leachim6/hello-world.git] 			bajar un proyecto a local

## Despaquetizar los Objects de un Pack 
cd .git/objects/
#cat [nombre pack] | git unpack-objects   			previamente hay que mover el pack y su indice a la carpeta objetos.

## Git IGNORE

.gitignore 										 se añaden todos los ficheros que se quedaran fueran del control de versiones.

## TAGS

#git tag v1.0.0 																	tag más lightweight
#git tag -a v2.0.0 -m "Tag guardada dentro de nuestro Git" 						crea objeto interno

## Trabajando con GitHub Repository

#git branch -a

#git remote -v
#git remote show origin 					Buscar *local out of date*
#git fetch
#git fetch -v
#git pull
#git pull -v
#git push

#git push --set-upstream origin fea3 			Cuando creas una nueva rama en local, la subes a origin repository

	Para borrar una rama de remoto:
	#git push origin :nombre-rama

#git reflog

## Pull request

#git rebase main
#git merge [rama] -v  					para ver que hay nuevo

## CHERRY PICK

#git cherry-pick {14193a7}    			traerte un commit especifico y hacerlo propio tuyo

## ALIAS LG para ver el Tree, time stamp y demas al git log

#git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr)%C(bold blue)<%an>%Creset' --abbrev-commit"




## PRACTICA 1
- Crear un nuevo proyecto Git "Practica"
- Añadir dos ficheros de texto mediante linea de comandos.
- Añadirlos como hash-objects
- Añadirlos con un tree 
- Hacer commit de los cambios eliminándolos después del stage.


100644 blob 1b3c2a41ee19149c2542d52a05bd271dbbf2f920	fichero1.txt
100644 blob 5b7c83d8a0c68cef85a46a28d2fc1008be015a59	fichero2.txt


cd......
mkdir Proyecto
cd Proyecto
git init
echo "fichero 1 " > fichero1.txt     				1b3c2a41ee19149c2542d52a05bd271dbbf2f920
echo "fichero 2 " > fichero2.txt 					5b7c83d8a0c68cef85a46a28d2fc1008be015a59
git hash-object fichero1.txt -w
git hash-object fichero2.txt -w

echo "100644 blob 1b3c2a41ee19149c2542d52a05bd271dbbf2f920	fichero1.txt
100644 blob 5b7c83d8a0c68cef85a46a28d2fc1008be015a59	fichero2.txt" > miArbol.txt

cat miArbol.txt | git mktree   						ea096cd2a52ed4e112210e070bbb18615e940e12
git read-tree ea096cd2a52ed4e112210e070bbb18615e940e12

Borras los ficheros de local

git checkout-index -a

git rm --cached fichero1.txt
git rm --cached fichero2.txt

## PRACTICA 2
Práctica Martes
- Sobre el proyecto "Practica" crear varios commits con modificaciones de ficheros
- Sed capaces de movernos entre commits y ver qué ficheros hay en qué commits
- Crear ramas directamente en el proyecto con diferentes commits
- Modificar un mismo fichero o varios en rama y en master para mergear conflictos
- Acceder a https://github.com/SilvanoGil/Spring5, forkear dicho proyecto a vuestro
GitHub, clonarlo, crear rama y modificar un fichero o crear un txt nuevo para 
mergearlo después al master principal en vuestro local
