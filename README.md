# student-linked-list
A C program that manages students using a singly linked list (add, remove, display). Simple example of dynamic memory and data structures.
#include <stdio.h>
#include <stdlib.h>
#include <string.h>


typedef struct etudiant etudiant;
struct etudiant {
    int id;
    char nom[50];
    etudiant *suivant;
};

typedef struct liste liste;
struct liste {
    int longueur;
    etudiant *tete;
};


liste *init(int id, char nom[50]){
    liste *L = malloc(sizeof(*L));
    etudiant *E = malloc(sizeof(*E));

    if (L == NULL || E == NULL)
        exit(EXIT_FAILURE);

    E->id = id;
    strcpy(E->nom, nom);
    E->suivant = NULL;

    L->tete = E;
    L->longueur = 1;

    return L;
}


void ajouterDebut(liste *L, int id, char nom[50]) {
    etudiant *E = malloc(sizeof(*E));
    if (E == NULL)
        exit(EXIT_FAILURE);

    E->id = id;
    strcpy(E->nom, nom);

    E->suivant = L->tete;
    L->tete = E;

    L->longueur++;
}


void ajouterFin(liste *L, int id, char nom[50]) {
    etudiant *E = malloc(sizeof(*E));
    if (E == NULL)
        exit(EXIT_FAILURE);

    E->id = id;
    strcpy(E->nom, nom);
    E->suivant = NULL;

    if (L->tete == NULL) {
        L->tete = E;
    } else {
        etudiant *tmp = L->tete;
        while (tmp->suivant != NULL)
            tmp = tmp->suivant;
        tmp->suivant = E;
    }

    L->longueur++;
}


void afficher(liste *L) {
    etudiant *tmp = L->tete;

    printf("La liste contient %d etudiants :\n", L->longueur);
    printf("---------------------------------\n");

    while (tmp != NULL) {
        printf("ID : %d | Nom : %s\n", tmp->id, tmp->nom);
        tmp = tmp->suivant;
    }
    printf("---------------------------------\n");
}


void supprimer(liste *L, int id) {
    if (L->tete == NULL) return;

    etudiant *tmp = L->tete;
    etudiant *prec = NULL;


    if (tmp->id == id) {
        L->tete = tmp->suivant;
        free(tmp);
        L->longueur--;
        return;
    }


    while (tmp != NULL && tmp->id != id) {
        prec = tmp;
        tmp = tmp->suivant;
    }

    if (tmp == NULL) return;

    prec->suivant = tmp->suivant;
    free(tmp);
    L->longueur--;
}


int main() {
    int choix;
    int id;
    char nom[50];

    liste *L = NULL;

    do {
        printf("\n===== MENU =====\n");
        printf("1 - Initialiser la liste (avec un etudiant)\n");
        printf("2 - Ajouter un etudiant au debut\n");
        printf("3 - Ajouter un etudiant a la fin\n");
        printf("4 - Supprimer un etudiant par ID\n");
        printf("5 - Afficher la liste\n");
        printf("0 - Quitter\n");
        printf("Votre choix : ");
        scanf("%d", &choix);
        getchar();

        switch (choix) {

        case 1:
            if (L != NULL) {
                printf("La liste est deja initialisee !\n");
                break;
            }
            printf("ID du premier etudiant : ");
            scanf("%d", &id);
            getchar();

            printf("Nom : ");
            fgets(nom, 50, stdin);
            nom[strcspn(nom, "\n")] = 0;

            L = init(id, nom);
            printf("Liste initialisee !\n");
            break;

        case 2:
            if (L == NULL) {
                printf("La liste n'est pas initialisee !\n");
                break;
            }
            printf("ID : ");
            scanf("%d", &id);
            getchar();

            printf("Nom : ");
            fgets(nom, 50, stdin);
            nom[strcspn(nom, "\n")] = 0;

            ajouterDebut(L, id, nom);
            printf("Ajoute au debut !\n");
            break;

        case 3:
            if (L == NULL) {
                printf("La liste n'est pas initialisee !\n");
                break;
            }
            printf("ID : ");
            scanf("%d", &id);
            getchar();

            printf("Nom : ");
            fgets(nom, 50, stdin);
            nom[strcspn(nom, "\n")] = 0;

            ajouterFin(L, id, nom);
            printf("Ajoute a la fin !\n");
            break;

        case 4:
            if (L == NULL) {
                printf("La liste n'est pas initialisee !\n");
                break;
            }
            printf("ID de l'etudiant a supprimer : ");
            scanf("%d", &id);

            supprimer(L, id);
            printf("Suppression terminee (si l'ID existait)\n");
            break;

        case 5:
            if (L == NULL) {
                printf("La liste n'est pas initialisee !\n");
                break;
            }
            afficher(L);
            break;

        case 0:
            printf("Au revoir !\n");
            break;

        default:
            printf("Choix invalide !\n");
        }

    } while (choix != 0);

    return 0;
}
