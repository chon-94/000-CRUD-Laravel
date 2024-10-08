# 000-CRUD-Laravel
 crud

# LARAVEL

## Instalar laravel
```bash
    composer create-project --prefer-dist laravel/laravel raiz "10.*"
```

### configurar '.env'
```ini
    DB_CONNECTION=mysql
    DB_HOST=127.0.0.1
    DB_PORT=3306
    DB_DATABASE=0CRUD
    DB_USERNAME=dev
    DB_PASSWORD=159357QWERTY
```

#### Migraciones laravel
```bash
    php artisan serve
```

## Crud

 Cuando ejecutas el comando `php artisan make:model Crud -mcr`, Laravel generará automáticamente tres archivos:

 1. **Modelo (`Crud.php`)**: Se crea el modelo para interactuar con la tabla correspondiente en la base de datos.
 2. **Controlador (`CrudController.php`)**: Se genera un controlador con los métodos básicos para el CRUD (create, store, edit, update, destroy, etc.).
 3. **Migración (`create_cruds_table.php`)**: Se genera una migración que contendrá la estructura de la tabla `cruds` en la base de datos.

  Te muestro cada uno de estos archivos:

### 1. Modelo `app/Models/Crud.php`
```php
    <?php

    namespace App\Models;

    use Illuminate\Database\Eloquent\Factories\HasFactory;
    use Illuminate\Database\Eloquent\Model;

    class Crud extends Model
    {
        use HasFactory;

        // Si deseas permitir asignación masiva, especifica los campos con fillable
        protected $fillable = [
            'campo1', 
            'campo2', 
            'campo3', // Añade los campos que necesites
        ];
    }
```

### 2. Controlador `app/Http/Controllers/CrudController.php`
```php
    <?php

    namespace App\Http\Controllers;

    use App\Models\Crud;
    use Illuminate\Http\Request;

    class CrudController extends Controller
    {
        // Método para listar los registros
        public function index()
        {
            $cruds = Crud::all(); // Obtiene todos los registros
            return view('crud.index', compact('cruds')); // Retorna a la vista index
        }

        // Método para mostrar el formulario de creación
        public function create()
        {
            return view('crud.create'); // Retorna a la vista create
        }

        // Método para almacenar un nuevo registro
        public function store(Request $request)
        {
            $request->validate([
                'campo1' => 'required',
                'campo2' => 'required',
                'campo3' => 'required',
            ]);

            Crud::create($request->all()); // Crea el registro con los datos del formulario

            return redirect()->route('crud.index')
                            ->with('success', 'Registro creado correctamente.');
        }

        // Método para mostrar un registro específico
        public function show(Crud $crud)
        {
            return view('crud.show', compact('crud')); // Retorna a la vista show
        }

        // Método para mostrar el formulario de edición
        public function edit(Crud $crud)
        {
            return view('crud.edit', compact('crud')); // Retorna a la vista edit
        }

        // Método para actualizar un registro existente
        public function update(Request $request, Crud $crud)
        {
            $request->validate([
                'campo1' => 'required',
                'campo2' => 'required',
                'campo3' => 'required',
            ]);

            $crud->update($request->all()); // Actualiza el registro con los datos nuevos

            return redirect()->route('crud.index')
                            ->with('success', 'Registro actualizado correctamente.');
        }

        // Método para eliminar un registro
        public function destroy(Crud $crud)
        {
            $crud->delete(); // Elimina el registro

            return redirect()->route('crud.index')
                            ->with('success', 'Registro eliminado correctamente.');
        }
    }
```

### 3. Migración `database/migrations/XXXX_XX_XX_create_cruds_table.php`
```php
    <?php

    use Illuminate\Database\Migrations\Migration;
    use Illuminate\Database\Schema\Blueprint;
    use Illuminate\Support\Facades\Schema;

    class CreateCrudsTable extends Migration
    {
        /**
        * Run the migrations.
        *
        * @return void
        */
        public function up()
        {
            Schema::create('cruds', function (Blueprint $table) {
                $table->id(); // Clave primaria
                $table->string('campo1'); // Ejemplo de campo
                $table->string('campo2'); // Ejemplo de campo
                $table->text('campo3'); // Ejemplo de campo
                $table->timestamps(); // Columnas created_at y updated_at
            });
        }

        /**
        * Reverse the migrations.
        *
        * @return void
        */
        public function down()
        {
            Schema::dropIfExists('cruds');
        }
    }
```

 Estos son los archivos que Laravel genera automáticamente. Puedes personalizar los campos en la migración y las validaciones en el controlador según tu proyecto.