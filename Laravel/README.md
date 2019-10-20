# Framework Laravel

## Séance 1: Installation de l'environnement de développement et création du premier projet.

### Mes prérequis

J'utilise: 
* PHPStrom comme IDE et pour visiualiser ma base de donnée
* PHP7
* Composer
* Uwamp pour le serveur web
* mysql que l'on relis à PHPStrom pour la base de donnée

### Créer son premier projet

* Création: 
`composer create-project --prefer-dist laravel/laravel [projet]`

* Lancement:
```
    pierre~>php artisan serve
    Laravel development server started: <http://127.0.0.1:8000>
```

Premier projet créé et lancé avec un premier visuel

![Visuel](images/visuel1.PNG)

* Connecter la base de données

Il faut modfier le fichier `.env`
```
    DB_CONNECTION=mysql
    DB_HOST=127.0.0.1
    DB_PORT=3306
    DB_DATABASE=laravel
    DB_USERNAME=root
    DB_PASSWORD=root
```

* Modification de la page d'accueil

La modification d'une page ce fait dans le dossier `ressource\views\`

Nous modifions `welcome.blade.php` qui est la page d'accueil et nous y ajoutons les routes vers de nouvelles pages que l'on vas créé

```
    <div class="content">
        <div class="title m-b-md">
            Yop
        </div>

        <div class="links">
            <a href="{{ url('/')}}">Docs</a>
            <a href="{{ url('/presentation')}}">Presentation</a>
            <a href="{{ route('contact' )}}">Contact</a>
            <a href="{{ route('autres' )}}">Autres</a>
        </div>
    </div>
```
Sans la prochaine étapes il y a un message d'erreur car nous n'avons pas encore définie les routes.

* Création de nouvelle page (Presentation/Contact/Autres)


Pour avoir de nouvelle page il suffit de créé des nouveaux fichiers dans le même dossier que `welcome.blade.php` avec l'exstension `.blade.php`

![New page](images/new-page1.PNG)

J'ai fait copier/coller de la page `welcome.blade.php` en changeant le titre

Ensuite il faut définir des routes vers ces pages dans `routes\web.php`

```
    <?php

    /*
    |--------------------------------------------------------------------------
    | Web Routes
    |--------------------------------------------------------------------------
    |
    | Here is where you can register web routes for your application. These
    | routes are loaded by the RouteServiceProvider within a group which
    | contains the "web" middleware group. Now create something great!
    |
    */

    Route::get('/', function () {
        return view('welcome');
    });
    Route::get('/contact', function () {
        return view('contact');
    })->name('contact');
    Route::get('/presentation', function () {
        return view('presentation');
    });
    Route::get('/autres', function () {
        return view('autres');
    })->name('autres');

```

Maintenant rafraichissons la page

![visuel2](images/visuel2.PNG)

![visuel3](images/visuel3.PNG)

* Créer un `UserTableSeeder` pour remplir la base de données de 100 utilisateurs

On utilise la commande ci-dssous, qui créera un fichier `database\seeds\UsersTableSeeder.php`

```
    pierre~> php artisan make:seeder UsersTableSeeder      
Seeder created successfully.
```

Nous rajoutons ensuite cette ligne dans notre fichier

`factory(App\User::class, 100)->create();`


UsersTableSeeder.php

```
    <?php

    use Illuminate\Database\Seeder;

    class UsersTableSeeder extends Seeder
    {
        /**
        * Run the database seeds.
        *
        * @return void
        */
        public function run()
        {
            factory(App\User::class, 100)->create();
        }
    }
```
Ne pas oublier de décomenter `$this->call(UsersTableSeeder::class);` dans `DatabaseSeeder.php`

Après modification on utilise la commande `composer autoload` pour généré nos utilisateur 

Vérification de la création des Users

![verif1](images/verif1.PNG)

## Séance numéro deux