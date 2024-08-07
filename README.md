# partielSC

## 1. Forkez ce repo ou suivez son exemple de format dans un repo que vous devez me partager à l'adresse remi.hamy@gmail.com
## Format du nom de repo PartielSoftwareCraftmanship"< Votre NOM PRENOM >"

## 2. Partie questions
Veuillez copier les questions et y répondre dans ce README pour les questions textes.
Pour les autres questions, mettez votre code dans le dossier CODE à la racine de votre repo


1) Corriger le code:

public class OrderSystem {
    private Map<Integer, List<String>> orderList; // Liste des commandes

    // On initialise orderList
    public OrderSystem() {
        orderList = new HashMap<>();
    }

    public void addNewOrder(Integer customerID, String itemName) {
        List<String> items = orderList.getOrDefault(customerID, new ArrayList<>());
        // On utilise getOrDefault pour obtenir une nouvelle liste d'item
        items.add(itemName);
        orderList.put(customerID, items); // Mise à jour de la liste des commandes pour le client
    }

    public void modifyOrder(Integer customerID, Integer itemIndex, String newItemName) {
        if (itemIndex < 0) {
            return; // On verifie que l'indice ne soit pas a 0
        }
        List<String> items = orderList.get(customerID);
        if (items != null && itemIndex < items.size()) {
            items.set(itemIndex, newItemName);
        }
    }

    public void removeOrder(Integer customerID, Integer itemIndex) {
        if (itemIndex < 0) {
            return;
        }
        List<String> items = orderList.get(customerID);
        if (items != null && itemIndex < items.size()) {
            items.remove(itemIndex);
        }
    }

    public void printOrders() {
        for (Map.Entry<Integer, List<String>> entry : orderList.entrySet()) {
            System.out.println("Customer ID: " + entry.getKey());
            System.out.println("Items: " + String.join(", ", entry.getValue()));
            System.out.println();
        }
    }
}


2) Qu'est ce que du code propre?

Du code propre c'est un code qui passe tout ses textes, qui exprime son intention tout en évitant de se répeter.
Qu'il y a le moins de classes, de fonctions ou de méthodes possibles.

3) De votre expérience de l’agilité en entreprise, en vous basant sur les piliers du manifeste vu en cours. Que pourriez vous améliorer dans la ges tion de vos projets ?

Dans la gestion de mes futurs projets, je pourrais améliorer premièrement la lecture de mon code, c'est a dire rendre mon code plus compréhensible à lire, en y ajoutant des commentaires pour faciliter le lecteur, faire des fonctions courte avec peu d'arguments. Essayer de comprendre avec mes collègues sur quoi ce disposer, faire soit de l'anglais soit du francais, eviter le franglais. 

4) Proposition de code alternative:

public class OrderProcessor {
    private Database database;
    private EmailService emailService;
    private InventorySystem inventorySystem;

    public OrderProcessor() {
        this.database = new Database();
        this.emailService = new EmailService();
        this.inventorySystem = new InventorySystem();
    }
    // Verifier les articles dans l'inventaire
    public void processOrder(Order order){
        checkInventory(order);
        database.saveOrder(order);
        sendOrderConfirmationEmail(order);
        updateInventory(order);
        applyDiscountIfApplicable(order);
    }

    private void checkInventory(Order order) throws ItemNotAvailableException {
        List<Item> items = order.getItems();
        for (Item item : items) {
            if (!inventorySystem.isItemAvailable(item)) {
                throw new RuntimeException("Item not available in inventory");
                }
        }
    }
    // Envoi d'un email de confirmation pour l'envoi de la commande
    private void sendOrderConfirmationEmail(Order order) {
        String message = "Your order has been received and is being processed.";
        emailService.sendEmail(order.getCustomerEmail(), "Order Confirmation", message);
    }

    // Mettre a jour l'inventaire
    private void updateInventory(Order order) {
        List<Item> items = order.getItems();
        for (Item item : items) {
            inventorySystem.updateInventory(item, item.getQuantity() * -1);
        }
    }
    // Appliquer une remise pour les clients fidèles
    private void applyDiscountIfApplicable(Order order) {
        if (order instanceof LoyalCustomerOrder) {
            ((LoyalCustomerOrder) order).applyDiscount();
        }
    }
}

public class LoyalCustomerOrder extends Order {
    @Override
    public void applyDiscount() {
        // Appliquer une remise de 10%
        setTotalPrice(getTotalPrice() * 0.9);
    }
}

4) Place au KATA:

   Créer une classe Survivant qui a une posi on (x, y) sur la carte, une orientati on (nord, sud, est, ouest), et un niveau de santé.

   public class Survivant {
     private int x;
     private int y;
     private String orientation (nord, sud, ouest, est);
     private int sante;

     public Survivant(int x, int y, String orientation, int sante) {
       this.x = x;
       this.y = y;
       this.orientation = orientation;
       this.sante = sante;
      }
    }


  Créer une classe Carte qui représente la zone que vous devez explorer. La carte est une grille de taille fixe (par exemple, 10x10, naviguer hors de ce e zone est fatal pour le Survivant).

  public class Carte {
    private int tailleX = 10;
    private int tailleY = 10;
    private char grille;

  public Carte() {
      // On crée la grille avec des caractères représentant la carte vide 
      grille = new char[tailleX][tailleY];
      for (int i = 0; i < tailleX; i++) {
          for (int j = 0; j < tailleY; j++) {
              grille[i][j] = '.';
          }
      }
  }

  
  Créer une classe Ressource qui représente les différents types de ressources que vous pouvez trouver sur la carte, comme de la nourriture, de l’eau et des armes.


  public class Ressource {
    private int nourriture;
    private int eau;
    private int arme;

  public Ressource (int nourriture, int eau, int arme) {
      this.nourriture = nourriture;
      this.eau = eau;
      this.arme = arme;
  }


  Créer une classe Zombie qui a une posi on (x, y) sur la carte et qui se déplace aléatoirement. Si le survivant entre en collision avec un zombie, il perd de la santé.

  public class Zombie {
    private int x;
    private int y;
    private Random random;

  public Zombie(int x, int y) {
      this.x = x;
      this.y = y;
      this.random = new Random();
  }

 public boolean collisionAvecSurvivant(Survivant survivant) {
        return x == survivant.getX() && y == survivant.getY();
    }

 
 Implémenter une méthode explorer() qui permet de se déplacer sur la carte en fonc on des commandes entrées par l’u lisateur (par exemple, “avancer”, “tourner à gauche”, “tourner à droite”).

 private void avancer() {
        switch (direction) {
            case "Nord":
                y--;
                break;
            case "Est":
                x++;
                break;
            case "Sud":
                y++;
                break;
            case "Ouest":
                x--;
                break;
            default:
                break;
        }
    }

  // Méthode pour tourner à gauche
    private void tournerAGauche() {
        switch (direction) {
            case "Nord":
                direction = "Ouest";
                break;
            case "Est":
                direction = "Nord";
                break;
            case "Sud":
                direction = "Est";
                break;
            case "Ouest":
                direction = "Sud";
                break;
            default:
                break;
        }
    }

  // Méthode pour tourner à droite
    private void tournerADroite() {
        switch (direction) {
            case "N":
                direction = "Est";
                break;
            case "E":
                direction = "Sud";
                break;
            case "S":
                direction = "Ouest";
                break;
            case "W":
                direction = "Nord";
                break;
            default:
                break;
        }
    }

  // Méthode pour explorer la carte en fonction des commandes
    public void explorer(String[] commandes) {
        for (String commande : commandes) {
            switch (commande.toLowerCase()) {
                case "avancer":
                    avancer();
                    break;
                case "tourner à gauche":
                    tournerAGauche();
                    break;
                case "tourner à droite":
                    tournerADroite();
                    break;
                default:
                    System.out.println("Commande non reconnue : " + commande);
                    break;
            }
        }
        System.out.println("Position finale du survivant : (" + x + ", " + y + "), Direction : " + direction);
    }

  
  Implémenter une méthode rencontrerZombie() qui permet de gérer les rencontres avec les zombies et de réduire la santé du survivant en fonc on du nombre de zombies rencontrés.

//méthode rencontrerZombie
  public void rencontrerZombie(int nombreZombies) {
        if (nombreZombies > 0) {
            zombiesRencontres += nombreZombies;
            int degats = nombreZombies * 10; // Par exemple, chaque zombie inflige 10 points de dégâts
            sante -= degats;
            System.out.println("Le survivant a rencontré " + nombreZombies + " zombies. Santé réduite de " + degats + " points.");
            System.out.println("Santé restante du survivant : " + sante);
        } else {
            System.out.println("Aucun zombie rencontré.");
        }
    }
 

 
