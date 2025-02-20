Voici les meilleures pratiques de nommage pour une application Node.js/Express :

### 1 Fichiers et Dossiers :

📁 controllers/          # Pluriel pour les dossiers <br/>
  📄 student.controller.js   # Singulier.type.js <br/>
📁 models/<br/>
  📄 student.model.js<br/>
📁 routes/<br/>
  📄 api.students.js    # Préfixe pour le type de route<br/>
📁 views/<br/>
  📁 partials/         # Pour les composants réutilisables<br/>
    📄 student-list.pug  # Kebab-case pour les vues

### 2 Fonctions de Base de Données (Queries) :

// Verbes d'action + nom de l'entité
getStudent()        // Récupérer un
getStudents()       // Récupérer plusieurs
createStudent()
updateStudent()
deleteStudent()
searchStudents()    // Actions spéciales

### 3 Contrôleurs :

// Action + Entité + Controller
listStudentsController
createStudentController
updateStudentController
deleteStudentController
searchStudentsController

### 4 Routes :

// RESTful conventions
GET    /students         // Liste
POST   /students         // Création
GET    /students/:id     // Détail
PUT    /students/:id     // Mise à jour complète
PATCH  /students/:id     // Mise à jour partielle
DELETE /students/:id     // Suppression
GET    /students/search  // Actions spéciales

### 5 Middlewares :

// Fonction + Middleware
authMiddleware
validateStudentMiddleware
errorHandlerMiddleware

### 6 Variables :

// camelCase descriptif
const studentList = [];
const currentUser = {};
const isAuthenticated = true;

### 7 Classes/Modèles :

// PascalCase pour les classes
class Student {
  // ...
}
class UserAuthentication {
  // ...
}

### 8 Constantes/Config :

// SNAKE_CASE majuscules
const MAX_LOGIN_ATTEMPTS = 3;
const DEFAULT_PAGE_SIZE = 10;

### 9 Méthodes de Modèle :

// Instance methods (camelCase)
student.getFullName()
student.calculateAge()

// Static methods (camelCase avec verbe descriptif)
Student.findByEmail()
Student.searchByName()

### 10 Services :

// Nom du service + Service
class EmailService {
  sendWelcomeEmail()
  sendPasswordReset()
}

### 11 Tests :

// Nom du test + Test
describe("createStudent", () => {
  it("should create a new student", () => {
    // ...
  });
});

describe('StudentController', () => {
  it('should create a new student', () => {
    // ...
  });
  it('should return 404 when student not found', () => {
    // ...
  });
});

### 12 Vues :

// PascalCase pour les composants
const StudentList = () => {
  // ...
};  

