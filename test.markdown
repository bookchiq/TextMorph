---
title: Migration de Wordpress vers Jekyll
wordpress_url: http://people.happycoders.org/dax/2009/11/09/migration-wordpress-jekyll
layout: post
tags: [wordpress, jekyll, ruby, web, blog]
categories: [geek, blog]
author: David
---
Les rares abonn������������s au flux RSS de mon blog l'auront remarqu������������, je ne
poste pas vraiment r������������guli������rement (j'esp������re m'am������������liorer, j'ai plein de
sujets de geek ������  partager :) ). En fait, jusqu'������  maintenant, je
passais plus de temps ������  mettre ������  jour
[Wordpress](http://www.wordpress.org) pour ������������viter d'������������tre ������ 
la traine niveau correction de failles de s������������cu. Si seulement c'������������tait
juste Wordpress, les plugins que j'utilisais devaient eux aussi subir
une mise ������  jour pour suivre l'������������volution des APIs de Wordpress. Et tout
ne se passait pas toujours tr������s bien :(.

Comme je suis dans ma p������������riode "revenons aux sources, pourquoi ai-je
besoin de m'encombrer avec ce #$%@!!. de XXX ?" (remplacer XXX par
Wordpress, Eclipse, OpenOffice ou tout autre soft qui finit
immanquablement en usine ������  gaz), et que je suis un peu un geek sur les
bords, je me suis dis que j'allais essayer
[Jekyll](http://github.com/mojombo/jekyll).

Qu'est ce que Jekyll ?
----------------------
C'est un petit outil ������������crit en Ruby qui ������  partir
de templates [Liquid](http://www.liquidmarkup.org/) et de fichiers
[Markdown](http://daringfireball.net/projects/markdown/syntax) (������a
ressemble ������  une syntaxe de Wiki : voyez le
[source de ce post](/dax/source/2009-11-09-migration-wordpress-jekyll.markdown))
g������������n������re un blog en statique que je d������������pose ensuite derri������re un Apache.

Par o������������ commencer ?
------------------
Je n'ai toutefois pas fait table rase, j'ai repris l'existant de mon
Wordpress en commen������ant par migrer le syst������me de commentaire sur le
service [Disqus](http://disqus.com) ������  l'aide du
[plugin Wordpress qui va bien](http://wordpress.org/extend/plugins/disqus-comment-system/).
Oui, un site avec des pages HTML en
statique va avoir du mal ������  conserver les commentaires des
posts! Je d������������l������gue donc, tout en me permettant une migration des
commentaires tout en douceur :

* Le plugin Disqus pour Wordpress a une fonction de migration,
* Le nouveau site inclus ensuite Disqus avec les quelques lignes de
  Javascript qui vont bien.

  Ensuite, un
  [petit script](http://github.com/mojombo/jekyll/blob/master/lib/jekyll/converters/wordpress.rb)
  permet d'extraire les posts de la base de
  donn������������es Wordpress pour les transformer en fichier Markdown. Le
  r������������sultat n'est pas parfait, mais ������a permet de d������������buter.

  Le templating
  -------------
  Une fois les posts migr������������s, il reste encore ������  ������������crire les templates dans
  lesquels les articles (et autres pages) s'ins������reront.

  Rien de magique, du HTML, du CSS, une pinc������������e de Javascript et un peu
  de [Liquid](http://www.liquidmarkup.org/) pour faire quelques boucles,
  quelques conditions :

  {% highlight html %}
  <div id="recent_posts" class="sidebar_section">
    <h4 class="sidebar_title">Recent posts</h4>
      <div class="sidebar_content">
          <ul>
	  	{% for post in site.posts limit:5 %}
			<li><a href="{{ post.url  }}">
			          {{ post.title }}
				  	</a></li>
						{% endfor %}
						    </ul>
						      </div>
						      </div>
						      {% endhighlight %}

						      Et donc ?
						      ---------
						      Oui, et donc ? Et bien je dirais que je me suis bien fait plaisir ;),
						      j'ai pass������������ un peu plus de temps que pr������������vu quand m������������me vu le niveau
						      HTML+CSS que j'avais.

						      Le blog est versionn������������ dans un [repo Git](/dax/projects/blog.git), donc
						      plus de probl������me de backup de base MySQL, de migration, de restauration
						      approximative, un ``git clone`` suivi de ``rake`` suffit (j'utilise
						      [Rake](http://)
						       pour g������������n������������rer quelques pages suppl������������mentaires, et lancer la g������������n������������ration
						        du site par Jekyll).

