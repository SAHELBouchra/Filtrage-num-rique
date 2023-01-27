# TP5-Filtrage-numérique

<a name="retour"></a>
## Sommaire :
1. [ Objectifs. ](#objectif)
2. [Filtrage de signaux simulés.](#part1)
3. [ Filtrage de sons. ](#part2)
4. [ Etude de quelques filtres numériques.](#part3)

<a name="objectif"></a>
### **1. Objectifs:**
 Améliorer la qualité de filtrage en augmentant l’ordre du filtre.
 Appliquer un filtre numérique pour supprimer les composantes indésirables d’un signal.
 Faire la synthèse d’un filtre numérique.

#### Commentaires :

 Il est à remarquer que ce TP traite en principe des signaux continus. Or, l'utilisation de Matlab suppose l'échantillonnage du signal. Il faudra donc être vigilant par rapport aux différences de traitement entre le temps continu et le temps discret.
 
#### Tracé des figures :
 
    Toutes les figures devront être tracées avec les axes et les légendes des axes appropriés.
 
#### Travail demandé :

    un script Matlab commenté contenant le travail réalisé et des commentaires sur ce que vous avez compris et pas compris, ou sur ce qui vous a semblé intéressant ou pas, bref tout commentaire pertinent sur le TP.
    
    $~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~$ [ (Revenir au sommaire) ](#retour)
***
 
   <a name="part1"></a>
### **2. Filtrage de signaux simulés.:**

####  **1- Créer un signal s(t) combinaison de deux signaux sinusoïdaux de fréquence différentes.**

```matlab
%%
% Durée du signal (en secondes)
T = 5;

% Fréquence d'échantillonnage (en Hz)
fs = 1000;

% Temps (en secondes)
ts = 1/fs;
t = 0:ts:1;

% Amplitude des deux signaux sinusoïdaux
A1 = 1;
A2 = 0.5;

% Fréquence des deux signaux sinusoïdaux (en Hz)
f1 = 100;
f2 = 200;

% Génération des deux signaux sinusoïdaux
s1 = A1*sin(2*pi*f1*t);
s2 = A2*sin(2*pi*f2*t);

% Combinaison des deux signaux pour obtenir le signal s(t)
s = s1 + s2;

% Affichage du signal s(t)
plot(t, s);
xlabel('Temps (s)');
ylabel('Amplitude');

```

<img width="812" alt="0" src="https://user-images.githubusercontent.com/93081417/215103698-76f613d5-1fc3-434a-bf37-2993fdb55fb4.png">

####  **2- Calculer et Tracer la TFD de ce signal.**

```matlab

% Calcul de la fréquence (en Hz) correspondant à chaque bin
f = (0:N-1)*fs/N;
fshift = (-N/2:N/2-1)*(fs/N);

y = fft(s);
plot(fshift,fftshift(abs(y)));
xlabel('Fréquence (Hz)');
ylabel('Amplitude');

```
<img width="818" alt="01" src="https://user-images.githubusercontent.com/93081417/215112224-f28afd34-1b22-4408-9005-048e16c978c8.png">

####  **3- Créer deux filtres permettant de séparer les deux signaux. Tracer la réponse fréquentielle des filtres numériques ainsi définis.**
####   ** Commandes : butter, cheby1, freqz**

