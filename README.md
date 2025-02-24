# Wardrobe-Management-System-
frontend 
cd wardrobe-management

php artisan serve

# Install Laravel Breeze for authentication
composer require laravel/breeze --dev
php artisan breeze:install
npm install && npm run dev
php artisan migrate

# Create models, migrations, and controllers for clothing items and categories
php artisan make:model ClothingItem -mcr
php artisan make:model Category -mcr

# Define migrations
cat <<EOT > database/migrations/2024_02_24_000000_create_categories_table.php
<?php
use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;
class CreateCategoriesTable extends Migration {
    public function up() {
        Schema::create('categories', function (Blueprint $table) {
            $table->id();
            $table->string('name');
            $table->timestamps();
        });
    }
    public function down() {
        Schema::dropIfExists('categories');
    }
}
EOT

cat <<EOT > database/migrations/2024_02_24_000001_create_clothing_items_table.php
<?php
use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;
class CreateClothingItemsTable extends Migration {
    public function up() {
        Schema::create('clothing_items', function (Blueprint $table) {
            $table->id();
            $table->string('name');
            $table->foreignId('category_id')->constrained();
            $table->timestamps();
        });
    }
    public function down() {
        Schema::dropIfExists('clothing_items');
    }
}
EOT

php artisan migrate

# Define API routes
cat <<EOT > routes/api.php
<?php
use Illuminate\Http\Request;
use Illuminate\Support\Facades\Route;
use App\Http\Controllers\ClothingItemController;
use App\Http\Controllers\CategoryController;
Route::middleware('auth:sanctum')->group(function () {
    Route::apiResource('categories', CategoryController::class);
    Route::apiResource('clothing-items', ClothingItemController::class);
});
EOT

# Serve the API
php artisan serve
