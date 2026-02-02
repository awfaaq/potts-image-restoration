# Restauration et segmentation conjointe d'images par le modèle de Potts ($L_0$)

Ce projet a été réalisé dans le cadre du **Master 1 Mathématique, Modélisation et apprentissage statistique (M1 MMAS)** de l'**Université Paris Cité**, pour le cours "bases pour le traitement d'images".

Il propose une implémentation et une analyse de l'algorithme présenté dans l'article de référence de **Storath et al. (2015)**, en se concentrant sur le problème du débruitage d'images constantes par morceaux (piecewise-constant Mumford–Shah problem).

## Article de référence

Ce travail est basé sur l'article suivant :

> **"Joint image reconstruction and segmentation using the Potts model"**
> *Martin Storath, Andreas Weinmann, Jürgen Frikel, and Michael Unser.*
> **Inverse Problems**, 31(2), 2015. [DOI: 10.1088/0266-5611/31/2/025003](https://doi.org/10.1088/0266-5611/31/2/025003)

### Apports théoriques de l'article intégrés au projet :
1.  **Splitting ADMM :** Décomposition du problème non-convexe 2D en sous-problèmes 1D solvables exactement.
2.  **Discrétisation isotrope (Système $\mathcal{N}_1$) :** L'article démontre que la discrétisation standard (horizontale/verticale) crée des artefacts de blocs ("staircasing"). Ce projet implémente le système de voisinage étendu incluant les **diagonales** avec des pondérations spécifiques ($\omega_s$) pour approcher la longueur euclidienne et améliorer l'isotropie.
3.  **Programmation dynamique :** Résolution des sous-problèmes 1D en temps polynomial $O(N^2)$.

## Objectif

Le modèle de Potts cherche à minimiser la fonctionnelle d'énergie suivante :

$$ u^* = \arg \min_u \left( \gamma \|\nabla u\|_0 + \|u - f\|_2^2 \right) $$

Où :
*   $f$ est l'image observée (bruitée).
*   $u$ est l'image restaurée (constante par morceaux).
*   $\|\nabla u\|_0$ est la pseudo-norme $L_0$ du gradient, pénalisant la longueur des discontinuités (sauts).
*   $\gamma$ est le paramètre de régularisation contrôlant la granularité de la segmentation.

*Note : L'article original traite le cas général des problèmes inverses ($Au - f$) incluant la tomographie. Ce projet se focalise sur le cas $A=Id$ (débruitage), simplifiant le terme d'attache aux données tout en conservant la complexité du terme de régularisation.*

## Méthodologie et algorithmes

Le projet implémente et compare plusieurs approches :

### 1. Solveur 1D exact (programmation dynamique)
Implémentation du cœur de la méthode de Storath. Cet algorithme trouve la segmentation optimale exacte d'un signal 1D bruité.

### 2. Approche 2D alternée (anisotrope)
Une méthode heuristique appliquant le solveur 1D itérativement sur les lignes et les colonnes. Bien que rapide, elle souffre d'anisotropie (artefacts alignés sur les axes).

### 3. Approche 2D ADMM (isotrope - système $\mathcal{N}_1$)
L'implémentation de l'algorithme de splitting proposé par Storath et al. pour réduire les artefacts géométriques :
*   Utilisation de la méthode des directions alternées (ADMM).
*   Couplage des 4 directions principales : horizontale, verticale, diagonale principale, anti-diagonale.
*   Application des poids optimaux dérivés dans l'article ($\omega_{h,v} = \sqrt{2}-1$, $\omega_{d} = 1-\frac{\sqrt{2}}{2}$) pour assurer une meilleure isotropie que la grille standard.

## Résultats principaux

Les tests effectués sur le fantôme de **Shepp-Logan** avec un bruit gaussien ($\sigma=0.1$) démontrent la supériorité de l'approche Potts ADMM :

| Méthode | PSNR (dB) | Observations |
| :--- | :--- | :--- |
| Filtre Gaussien ($\sigma=1.5$) | 18.84 | Flou important sur les bords. |
| Filtre Médian ($3\times3$) | 24.38 | Bruit résiduel ("tremblement") dans les zones plates. |
| Potts 2D (Alterné) | 31.85 | Très net, mais artefacts directionnels ("effet Manhattan"). |
| **Potts 2D (ADMM)** | **33.34** | **Meilleure reconstruction, contours nets et géométrie préservée.** |

## Installation et utilisation

Le projet est présenté sous forme d'un notebook Google Colab.

**Prérequis :**
```bash
pip install numpy matplotlib scipy scikit-image
```

**Structure du code :**
*   `min_l2_potts_1d` : Solveur 1D par programmation dynamique.
*   `potts_2d_admm_final` : Algorithme principal utilisant les poids isotropes avec diagonales.
*   Comparaison avec `scipy.ndimage.gaussian_filter` et `median_filter`.

## 👥 Auteur

*   **Daniel ASHRAFUL**


---
*Ce projet illustre comment des méthodes d'optimisation non-convexes peuvent surpasser les filtres linéaires classiques pour la restauration d'images structurées.*
