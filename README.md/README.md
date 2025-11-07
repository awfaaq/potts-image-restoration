# Restauration d'images avec le modèle de Potts (Implémentation de Storath et al.)

Ce projet, réalisé dans le cadre du Master 1 MMAS, est une exploration pédagogique de l'algorithme de restauration d'images basé sur le modèle de Potts, en suivant l'approche décrite par M. Storath et al.

> **Référence principale :**
> Storath, M., Weinmann, A., Frikel, J., & Unser, M. (2015). *Joint image reconstruction and segmentation using the Potts model*. Inverse Problems, 31(2), 025003.

## Contexte

Ce projet aborde le problème du **débruitage**, un cas fondamental de problème inverse, en exploitant l'a priori que les images d'intérêt sont **constantes par morceaux**. Nous implémentons et analysons un algorithme de l'état de l'art basé sur la Méthode des directions alternées des multiplicateurs (ADMM) avec une discrétisation isotrope.

## Structure du projet

- **/report/** : Contient le rapport de recherche détaillé au format PDF, qui expose le cadre mathématique, l'algorithmique et l'analyse des résultats.
- **/notebook/** : Contient le Notebook Jupyter `projet_potts.ipynb` avec l'implémentation complète du code, les expériences et les visualisations.
- **/images/** : Contient les images de test utilisées (scanner CT cérébral) et des exemples de résultats.

## Aperçu des résultats

L'algorithme a été testé sur le fantôme de Shepp-Logan, un cas d'étude idéal pour les images constantes par morceaux.

![Comparaison des méthodes de débruitage](images/resultat_comparaison.png)


Le tableau ci-dessous résume les performances quantitatives (PSNR en dB).

| Méthode | Filtre Gaussien | Filtre Médian | Potts (Alterné) | Potts (ADMM) |
| :--- | :---: | :---: | :---: | :---: |
| **PSNR (dB)** | 18.84 | 24.38 | **31.85** | 29.24 |

Les résultats montrent la nette supériorité des méthodes basées sur le modèle de Potts par rapport aux filtres classiques.

## Comment utiliser ce projet

### Prérequis

- Python 3.x
- Les bibliothèques listées dans `requirements.txt`.

### Installation

1.  Clonez ce dépôt :
    ```bash
    git clone https://github.com/VOTRE_NOM_UTILISATEUR/potts-image-restoration.git
    cd potts-image-restoration
    ```
2.  Installez les dépendances :
    ```bash
    pip install -r requirements.txt
    ```

### Exécution

Ouvrez et exécutez le notebook `notebook/projet_potts.ipynb` en utilisant Jupyter Notebook ou JupyterLab.

```bash
jupyter notebook notebook/projet_potts.ipynb
```

## Auteurs

- **Almahboub Fouad**
- **Daniel Ashraful**

Sous la supervision de **Georges Koepfler**.
Projet de Master 1 Mathématiques et Applications (MMAS).
