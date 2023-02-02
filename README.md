# TP5-Filtrage-numérique

<a name="retour"></a>
## Sommaire :
1. [ Objectifs. ](#objectif)
2. [Filtrage de signaux simulés.](#part1)
3. [ Filtrage de sons. ](#part2)
4. [ Etude de quelques filtres numériques.](#part3)

<a name="objectif"></a>
### **1. Objectifs:**
• Améliorer la qualité de filtrage en augmentant l’ordre du filtre.
• Appliquer un filtre numérique pour supprimer les composantes indésirables d’un signal.
• Faire la synthèse d’un filtre numérique.

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
####   ** • Commandes : butter, cheby1, freqz**
#### Avec "butter"
```matlab
% Paramètres du filtre passe-bas
fcut = 110; % fréquence de coupure en Hz
ordre = 5; % ordre du filtre
% Création du filtre passe-bas
[b, a] = butter(ordre, fcut/(fs/2), 'low'); % la fréquence de coupure normalisée pour être comprise entre 0 et 1
% Tracer la réponse fréquentielle du filtre
 %freqz(b, a);
 ```
![freqz](https://user-images.githubusercontent.com/121026639/216441764-5a5cfd75-eb56-49c4-8fb3-f10f5d7ff4fb.png)
#### Avec "chepy1"
```matlab
% Paramètres du filtre
f_coupure = 190; % Fréquence de coupure (en Hz)
ordre = 5; % Ordre du filtre
ripple = 0.5; % Ripple en dB

% Création du filtre passe-bas
[b, a] = cheby1(ordre, ripple, f_coupure/(fs/2), 'high');
 freqz(b, a);
 ```
![freqz2](https://user-images.githubusercontent.com/121026639/216442004-c6ab4a14-3ce5-423a-b3c1-7a964c649dee.png)


####  **4- Calculer les signaux de sortie des filtres pour le signal d’entrée s(t).**
####   ** • Commandes : filter**
##### Le premier filtre

```matlab
% Appliquer le filtre passe-bas au signal s(t)
x_filtre = filter(b, a, x);
% Calculer la TFD du signal
fft_x_filtre = fft(x_filtre);

% Obtenir la fréquence
f = (0:N-1)*fs/N;
fshift = (-N/2:N/2-1)*(fs/N);
plot(fshift,fftshift(abs(fft_x_filtre)));
title("le spectre filtré par butter");
```
![butter](https://user-images.githubusercontent.com/121026639/216442786-40b99cf7-b00b-4c0f-a308-b4ad513c42eb.png)

##### Le deuxième filtre
```matlab
% Appliquer le filtre passe-bas au signal s(t)
x_filtre = filter(b, a, x);
%plot(t,x_filtre)
% Calculer la TFD du signal
fft_x_filtre = fft(x_filtre);

% Obtenir la fréquence
f = (0:N-1)*fs/N;
fshift = (-N/2:N/2-1)*(fs/N);
plot(fshift,fftshift(abs(fft_x_filtre)));
title("le spectre filtré par cheby1");
```
![chepy1](https://user-images.githubusercontent.com/121026639/216443022-52df111e-26db-4178-b72a-671039ad166f.png)

####  **5- Avec quelles caractéristiques de filtres (fréquence de coupure et ordre) obtient-on de bonnes performances de filtrage ?**
##### Le choix de fréquence de coupure pour le premier filtre est 110 Hz et d'ordre 5
##### Et pour le deuxième filtre la fréquence de coupure est 190 Hz et d'ordre 5
