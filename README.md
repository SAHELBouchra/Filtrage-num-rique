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
### **Filtrage de signaux simulés.:**

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
 ##### ==> la fonction "butter" est utilisée pour générer les coefficients d'un filtre passe-bas ou passe-haut. Cette fonction peut être utilisée pour créer un filtre numérique de Butterworth à n poles, qui est un filtre de type IIR (réponse impulsionnelle finie)
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
 ##### ==> la fonction "cheby1" est utilisée pour générer les coefficients d'un filtre passe-bas ou passe-haut à réponse en cheminement à faible ripple. Cette fonction peut être utilisée pour créer un filtre numérique de premier ordre, qui est un filtre de type IIR (réponse impulsionnelle finie).
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
##### ==> Le choix de fréquence de coupure pour le premier filtre est 110 Hz et d'ordre 5
##### ==> Et pour le deuxième filtre la fréquence de coupure est 190 Hz et d'ordre 5

### **Filtrage de sons:**

####  **1- Télécharger les fichiers à traiter (akesuper.wav et affreux.wav).**
```matlab
[x,fe] = audioread('affreux.wav');
[x2,fe2] = audioread('akesuper.wav');
```

####  **2- Effectuer une analyse fréquentielle de chaque signal.**

```matlab
N1 = length(x);
N2 = length(x2);
te = 1/fe;
t1 = (0:N1-1)*te;
t2 = (0:N2-1)*te;
subplot(2,1,1)
plot(t1,x)
title("affreux");
subplot(2,1,2)
plot(t2,x2)
title("akesuper");

f1 =(0:N1-1)*(fe/N1); %frequence du spectre
 fshift1 = (-N1/2:N1/2-1)*(fe/N1);
 
 f2 =(0:N2-1)*(fe/N1); %frequence du spectre
 fshift2 = (-N2/2:N2/2-1)*(fe/N2);
subplot(2,1,1)
 y = fft(x); % y: spectre , fft(x) : transformee de fourier
 plot(fshift1,fftshift(abs(y)));
 title("affreux");
 subplot(2,1,2)
  y2 = fft(x2); % y: spectre , fft(x) : transformee de fourier
 plot(fshift2,fftshift(abs(y2)));
  title("akesuper");
```
![sounds](https://user-images.githubusercontent.com/121026639/216446629-59db8272-75a8-437f-afe0-36a1a9dc77cb.png)

![spectres_audios](https://user-images.githubusercontent.com/121026639/216447546-c82f19ef-ee25-406a-a41d-acd2c30570a2.png)


####  **3- Restauration de sons**
####  **- Superposer un bruit de faible amplitude au signal en utilisant la commande randn.**
```matlab
bruit1 = randn(size(x))*10^(-1);
bruit2 = randn(size(x2))*10^(-1);% génération d'un bruit blanc gaussien de -20dB
y_bruit = x + bruit1; % superposition du bruit au signal
y2_bruit= x2 + bruit2 ;
```
####  **- Proposer ensuite un filtre numérique.**
##### ==> On va utiliser "butter"

####  ** - Tracer la réponse fréquentielle de ce filtre (diagramme de bode).**

```matlab
freqz(b, a);
```

![f](https://user-images.githubusercontent.com/121026639/216449178-6c3daa15-a70a-4b77-82a7-e7716864cf33.png)

####  ** - Calculer la sortie du filtre.**
```matlab
 % Design du filtre
 fcut = 2500; % fréquence de coupure en Hz
ordre = 10; % ordre du filtr
[b,a] = butter(ordre, fcut/(fe/2), 'low'); % Filtre passe-bas de 10ème ordre avec une coupure à 0.3*fe

% Filtrage du signal bruité
y_filtre = filter(b,a,y_bruit);
```
![signals](https://user-images.githubusercontent.com/121026639/216449164-43d30587-5ccf-4cdf-9236-cdea958aa7b8.png)

####  **4- Analyser le résultat obtenu en fonction de la fréquence de coupure du filtre L’enregistrement sous format WAV d’un signal se fait avec la commande wavwrite**
```matlab
%enregistrement du signal filtré
audiowrite('audy_filtre.wav',y_filtre, fe);
```

<a name="part2"></a>
### **Etude de quelques filtres numériques**
####  **1- Parmi les filtres IIR suivants, lequel est stable ? (Utiliser la commande roots et la commande tf2zpk, zplane)**
####  **a. B=[0.0725 0.2200 0.4085 0.4883 0.4085 0.2200 0.0725],**
####     **A=[1.0000 -0.5835 1.7021 -0.8477 0.8401 -0.2823 0.0924]**
####  **b. B=[1.0000 1.3000 0.4900 -0.0130 -0.0290],**
####     **A=[1.0000 -0.4326 -1.6656 0.1253 0.2877]**
####  **c. B=[1.0000 -1.4000 0.2400 0.3340 -0.1305],**
####     **A=[1.0000 0.5913 -0.6436 0.3803 -1.0091]**

 ##### ==>La fonction "tf2zpk" permet de convertir une fonction de transfert d'un système continu, en une fonction de transfert discrète.
 ##### ==>La fonction "zplane" est utilisée pour visualiser les zéros et les poles d'un filtre. Cela permet de déterminer la stabilité et les caractéristiques de fréquence d'un filtre, il affiche un diagramme de Nyquist représentant les zéros et les poles dans le plan complexe. Dont,Les zéros sont représentés par des cercles blancs et les poles par des cercles noirs. La stabilité d'un filtre peut être déterminée en examinant les positions des poles par rapport à l'unité (dans ou en dehors du cercle unité).

#####  Pour a:

```matlab
% Coefficients du filtre
B = [0.0725 0.2200 0.4085 0.4883 0.4085 0.2200 0.0725];
A = [1.0000 -0.5835 1.7021 -0.8477 0.8401 -0.2823 0.0924];
% Racines du polynôme caractéristique
r = roots(B);
% Vérification de la stabilité
isStable = all(real(r) <0);
if isStable 
    disp("Le filtre 1 est stable.")
else
    disp("Le filtre 1 n'est pas stable.")
end
% Utiliser la commande tf2zpk pour convertir le filtre en forme polaire
[z,p,k] = tf2zpk(B,A);   %"z" représente les zéros du filtre  %"p" représente les pôles du filtre  %"k" représente le gain constant du filtre.
% Utiliser la commande zplane pour tracer le plan de pole-zero
zplane(z,p);
```
  ![1](https://user-images.githubusercontent.com/121026639/216456066-d23a3dae-298f-485a-9154-5042dd95284a.png)
##### ==> le filtre est stable.
#####  Pour b:  
  ```matlab
% Coefficients du filtre
B=[1.0000 1.3000 0.4900 -0.0130 -0.0290];
A=[1.0000 -0.4326 -1.6656 0.1253 0.2877];
% Racines du polynôme caractéristique
r = roots(B);
% Vérification de la stabilité
isStable = all(real(r) <0);
if isStable 
    disp("Le filtre 2 est stable.")
else
    disp("Le filtre 2 n'est pas stable.")
end
% Utiliser la commande tf2zpk pour convertir le filtre en forme polaire
[z,p,k] = tf2zpk(B,A);
% Utiliser la commande zplane pour tracer le plan de pole-zero
zplane(z,p);
```
  ![2](https://user-images.githubusercontent.com/121026639/216456609-6bc8fe14-e8d9-4c83-97d1-d473536a08cf.png)
##### ==> le filtre n'est pas stable.
#####  Pour c: 
```matlab
% Coefficients du filtre

B=[1.0000 -1.4000 0.2400 0.3340 -0.1305];
A=[1.0000 0.5913 -0.6436 0.3803 -1.0091];

% Racines du polynôme caractéristique
r = roots(B);

% Vérification de la stabilité

isStable = all(real(r) <0);

if isStable 
    disp("Le filtre 3 est stable.")
else
    disp("Le filtre 3 n'est pas stable.")
end
% Utiliser la commande tf2zpk pour convertir le filtre en forme polaire
[z,p,k] = tf2zpk(B,A);

% Utiliser la commande zplane pour tracer le plan de pole-zero
zplane(z,p);
```
![3](https://user-images.githubusercontent.com/121026639/216457847-4f9c4473-2f1c-492f-a86e-28715c35fbc0.png)
##### ==> le filtre n'est pas stable.

####  **2- Vérifier que votre sélection de filtre stable est correcte en utilisant la fonction filter et en appliquant au filtre l’impulsion de Dirac comme signal d'entrée (Voir comment définir le signal impulsion de Dirac dans le cas discret). Essayer ceci sur l'un des filtres instables et observer la différence avec le filtre stable. Utiliser également la commande impz.**

```matlab
B=[0.0725 0.2200 0.4085 0.4883 0.4085 0.2200 0.0725];
A=[1.0000 -0.5835 1.7021 -0.8477 0.8401 -0.2823 0.0924];

% Définir l'impulsion de Dirac
d = [1, zeros(1,1000)]; % signal d'entrée

% Appliquer le filtre au signal d'entrée
y = filter(A,B, d);
N=length(d);
fe=1000;
te=1/fe;
% Utiliser la commande impz pour afficher les premières réponses impulsionnelles
impulsion=impz(A,B,100)

if abs(impulsion(end)) < 1e-5
    fprintf('Le filtre est stable\n');
else
    fprintf('Le filtre n''est pas stable\n');
end
```

####  **3- Ecrire une fonction « transfert » qui calculera la fonction de transfert (transmittance en Z du filtre) d'un filtre IIR, et puis qui évalue cette transmittance
en différentes valeurs de fréquence. L'entrée devra être les vecteurs B et A (coefficients du filtre), le nombre d’échantillons, ainsi que la période d’échantillonage. La sortie devra être la fonction de transfert complexe et ses valeurs pour les fréquences correspondantes.**

####  **• Commandes : tf, evalfr. Rappel : z = exp(j*2*pi*f*te)**

```matlab
function [H,H_f, freqs] = transfert(B, A, N, T)
    % Calculer la fonction de transfert
    H = tf(B, A);
   
    % Calculer les fréquences pour lesquelles évaluer la transmittance
    f = linspace(0, 1/(2*T), N/2);
    freqs = f;
    
    % Évaluer la transmittance pour chaque fréquence
    for i = 1:length(f)
        z = exp(1i*2*pi*f(i)*T);
        H_f(i) = evalfr(H,z);
    end
end
```
####  **4- Calculer le transformée de Fourier de la réponse impulsionnelle du filtre stable (revoir la question 1), et puis tracer son module en dB et sa phase.**
####  **•Nombre d’échantillons = 10000, te = 1e-4.**
```matlab
B=[0.0725 0.2200 0.4085 0.4883 0.4085 0.2200 0.0725];
A=[1.0000 -0.5835 1.7021 -0.8477 0.8401 -0.2823 0.0924];
N = 10000; % Nombre d'échantillons
Ts = 1e-4; % Période d'échantillonage
% Réponse impulsionnelle du filtre
h = impz(B, A, N);

% Transformée de Fourier de la réponse impulsionnelle
H = fft(h);

% Fréquences correspondantes
f = linspace(0, 1/(2*Ts), N/2+1);

% Tracer le module en dB
subplot(2,1,1)

plot(f, 20*log10(abs(H(1:N/2+1))));
xlabel('Fréquence (Hz)');
ylabel('Module en dB');

% Tracer la phase
subplot(2,1,2)
plot(f, angle(H(1:N/2+1)));
xlabel('Fréquence (Hz)');
ylabel('Phase (rad)');
```

![bode](https://user-images.githubusercontent.com/121026639/216459902-849f58de-de8a-47fa-811e-ae95c9fadb67.png)


####  **5- Calculer la transmittance en Z de ce filtre sur le vecteur fréquences défini précédemment, en faisant appel à la fonction « transfert », et puis tracer son
module en Db et sa phase. Comparer ce dernier avec celui obtenu dans la question précédente. Conclure.**
```matlab
[H,H_f,w] = transfert(B,A,N,te);
% Tracer le module en dB
subplot(2,1,1)
plot(w, 20*log10(abs(H_f)));
xlabel('Fréquence (Hz)');
ylabel('Module en dB');

% Tracer la phase
subplot(2,1,2)
plot(w, angle(H_f));
xlabel('Fréquence (Hz)');
ylabel('Phase (rad)');
```
![z](https://user-images.githubusercontent.com/121026639/216460405-d2d70501-9f33-4ebd-be3e-2daf1695254b.png)

####  **6- Utiliser la commande firpm pour concevoir un filtre FIR ayant les spécifications suivantes :**
####  **•Bande passante : jusqu’à 500Hz.**
####  **•Bande atténuante : à partir de 570Hz.**
####  **•Fréquence d’échantillonnage : 2000Hz.**
####  **•Atténuation minimale dans la bande atténuante : -45dB.**
####  **•Atténuation maximale dans la bande passante : -0.2dB.**
