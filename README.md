#include <stdio.h>
#include <stdlib.h>
#include <string.h>

// Structure pour représenter un voyageur
typedef struct {
    char nom[50];
    char prenom[50];
    int age;
} Voyageur;

// Structure pour représenter un bus
typedef struct {
    int numero;
    char destination[50];
} Bus;

// Structure pour représenter une réservation
typedef struct {
    int numero_reservation;
    Bus bus;
    Voyageur voyageur;
} Reservation;

// Structure pour représenter un nœud de liste chaînée
typedef struct Node {
    Reservation reservation;
    struct Node* next;
} Node;

// Structure pour représenter une pile
typedef struct {
    Node* top;
} Stack;

// Structure pour représenter une file
typedef struct {
    Node* front;
    Node* rear;
} Queue;

// Fonction pour initialiser une pile
void initStack(Stack* stack) {
    stack->top = NULL;
}

// Fonction pour empiler un élément dans la pile
void push(Stack* stack, Reservation reservation) {
    Node* newNode = (Node*)malloc(sizeof(Node));
    newNode->reservation = reservation;
    newNode->next = stack->top;
    stack->top = newNode;
}

// Fonction pour dépiler un élément de la pile
Reservation pop(Stack* stack) {
    if (stack->top == NULL) {
        Reservation emptyReservation;
        emptyReservation.numero_reservation = -1; // Valeur pour indiquer une pile vide
        return emptyReservation;
    }
    Node* temp = stack->top;
    Reservation poppedReservation = temp->reservation;
    stack->top = temp->next;
    free(temp);
    return poppedReservation;
}

// Fonction pour initialiser une file
void initQueue(Queue* queue) {
    queue->front = queue->rear = NULL;
}

// Fonction pour ajouter un élément à la file
void enqueue(Queue* queue, Reservation reservation) {
    Node* newNode = (Node*)malloc(sizeof(Node));
    newNode->reservation = reservation;
    newNode->next = NULL;
    if (queue->rear == NULL) {
        queue->front = queue->rear = newNode;
        return;
    }
    queue->rear->next = newNode;
    queue->rear = newNode;
}

// Fonction pour retirer un élément de la file
Reservation dequeue(Queue* queue) {
    if (queue->front == NULL) {
        Reservation emptyReservation;
        emptyReservation.numero_reservation = -1; // Valeur pour indiquer une file vide
        return emptyReservation;
    }
    Node* temp = queue->front;
    Reservation dequeuedReservation = temp->reservation;
    queue->front = temp->next;
    if (queue->front == NULL) {
        queue->rear = NULL;
    }
    free(temp);
    return dequeuedReservation;
}

// Fonction pour afficher le menu
void afficherMenu() {
    printf("\nMenu:\n");
    printf("1. Creer un nouveau compte voyageur\n");
    printf("2. Creer une nouvelle reservation\n");
    printf("3. Afficher les details des bus\n");
    printf("4. Afficher et modifier les details d'une reservation\n");
    printf("5. Afficher et modifier les details du voyageur\n");
    printf("6. Ajouter une fonctionnalite supplementaire\n");
    printf("7. Afficher le contenu de la base\n");
    printf("8. Quitter\n");
}

// Fonction pour créer un nouveau compte voyageur
Voyageur creerNouveauVoyageur() {
    Voyageur nouveauVoyageur;
    printf("Entrez le nom du voyageur: ");
    scanf("%s", nouveauVoyageur.nom);
    printf("Entrez le prenom du voyageur: ");
    scanf("%s", nouveauVoyageur.prenom);
    printf("Entrez l'age du voyageur: ");
    scanf("%d", &nouveauVoyageur.age);
    return nouveauVoyageur;
}

// Fonction pour créer une nouvelle réservation
Reservation creerNouvelleReservation() {
    Reservation nouvelleReservation;
    printf("Entrez le numero du bus: ");
    scanf("%d", &nouvelleReservation.bus.numero);
    printf("Entrez la destination du bus: ");
    scanf("%s", nouvelleReservation.bus.destination);
    nouvelleReservation.voyageur = creerNouveauVoyageur();
    return nouvelleReservation;
}

// Fonction pour afficher les détails d'un bus
void afficherDetailsBus(Bus bus) {
    printf("Numero du bus: %d\n", bus.numero);
    printf("Destination: %s\n", bus.destination);
}

// Fonction pour afficher les détails d'une réservation
void afficherDetailsReservation(Reservation reservation) {
    printf("Numero de reservation: %d\n", reservation.numero_reservation);
    afficherDetailsBus(reservation.bus);
    printf("Details du voyageur:\n");
    printf("Nom: %s\n", reservation.voyageur.nom);
    printf("Prenom: %s\n", reservation.voyageur.prenom);
    printf("Age: %d\n", reservation.voyageur.age);
}

// Fonction pour afficher les détails d'un voyageur
void afficherDetailsVoyageur(Voyageur voyageur) {
    printf("Nom: %s\n", voyageur.nom);
    printf("Prenom: %s\n", voyageur.prenom);
    printf("Age: %d\n", voyageur.age);
}

// Fonction pour sauvegarder une réservation dans un fichier
void sauvegarderReservation(Reservation reservation, FILE* file) {
    fwrite(&reservation, sizeof(Reservation), 1, file);
}

// Fonction pour charger une réservation depuis un fichier
Reservation chargerReservation(FILE* file) {
    Reservation loadedReservation;
    fread(&loadedReservation, sizeof(Reservation), 1, file);
    return loadedReservation;
}

// Fonction principale
int main() {
    Stack stack;
    initStack(&stack);

    Queue queue;
    initQueue(&queue);

    FILE* reservationsFile = fopen("reservations.dat", "ab+");
    if (reservationsFile == NULL) {
        printf("Erreur lors de l'ouverture du fichier de réservations.\n");
        return 1;
    }

    int choix;
    do {
        afficherMenu();
        printf("Choisissez une action (1-8): ");
        scanf("%d", &choix);

        switch (choix) {
            case 1: // Creer un nouveau compte voyageur
                push(&stack, creerNouveauVoyageur());
                break;
            case 2: // Creer une nouvelle reservation
                enqueue(&queue, creerNouvelleReservation());
                break;
            case 3: // Afficher les details des bus
                // Implémenter la logique pour afficher les détails des bus
                break;
            case 4: // Afficher et modifier les details d'une reservation
                // Implémenter la logique pour afficher et modifier les détails d'une reservation
                break;
            case 5: // Afficher et modifier les details du voyageur
                // Implémenter la logique pour afficher et modifier les détails du voyageur
                break;
            case 6: // Ajouter une fonctionnalite supplementaire
                // Ajouter ici une nouvelle fonctionnalité
                break;
            case 7: // Afficher le contenu de la base
                rewind(reservationsFile);
                Reservation loadedReservation;
                printf("\nContenu de la base:\n");
                while (fread(&loadedReservation, sizeof(Reservation), 1, reservationsFile) == 1) {
                    afficherDetailsReservation(loadedReservation);
                }
                break;
            case 8: // Quitter
                printf("Merci d'avoir utilise E-Bus. Au revoir!\n");
                break;
            default:
                printf("Choix invalide. Veuillez entrer un nombre entre 1 et 8.\n");
        }

        // Sauvegarder les réservations dans le fichier
        rewind(reservationsFile);
        while (!feof(reservationsFile)) {
            Reservation temp = dequeue(&queue);
            if (temp.numero_reservation != -1) {
                sauvegarderReservation(temp, reservationsFile);
            }
        }

        // Libérer la mémoire utilisée par la pile
        Node* tempNode;
        while (stack.top != NULL) {
            tempNode = stack.top;
            stack.top = tempNode->next;
            free(tempNode);
        }

    } while (choix != 8);

    fclose(reservationsFile);

    return 0;
}
