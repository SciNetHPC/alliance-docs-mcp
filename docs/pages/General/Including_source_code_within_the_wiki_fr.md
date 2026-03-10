---
title: "Including source code within the wiki/fr"
url: "https://docs.alliancecan.ca/wiki/Including_source_code_within_the_wiki/fr"
category: "General"
last_modified: "2016-05-19T21:57:27Z"
page_id: 143
display_title: "Inclure du code source dans le wiki"
language: "fr"
---

__NOTOC__

Pour inclure du code source dans le wiki, nous utilisons l'extension SyntaxHighlight_GeSHi. Vous pouvez facilement inclure un extrait de code source grâce à la balise  .

== Options de la balise  ==
Pour la liste des options, veuillez vous référer à cette page.

=== Option lang ===
L'option lang permet de définir le langage utilisé pour la détection de la syntaxe. Le langage par défaut, si ce paramètre est omis, est le C++. La liste des langages supportés est disponible ici.

=== Option line ===
L'option line permet d'afficher des numéros de ligne.

== Exemple ==
Voici un exemple de code C++ créé avec la balise  ... .

#include
#include
#include
#include
using namespace std;

void flushIfBig(ofstream & out, ostringstream & oss, int size, bool force=false) {
	if (oss.tellp() >= size) {
		out << oss.str();
		oss.str(""); //reset buffer
	}
}
int main() {
	int buff_size = 50*1024*1024;

ofstream out ("file.dat");
	ostringstream oss (ostringstream::app);
	oss.precision(5);
	for (int i=0; i<100*buff_size; i++)
	{
		oss << i << endl;
		flushIfBig(out,oss,buff_size);
	}
	flushIfBig(out,oss,buff_size,true);
	out.close();
}