


# Using MongoDB for Audits in Laravel

You can leverage MongoDB to store your audits table in a Laravel application. Follow these steps to set up MongoDB for auditing:

## Steps

1. **Install MongoDB PHP Drivers**
   - Download the drivers from [MongoDB PHP Driver Releases](https://github.com/mongodb/mongo-php-driver/releases/tag/1.16.0).

2. **Install MongoDB Compass or Use MongoDB Atlas**
   - Choose either MongoDB Compass or MongoDB Atlas for managing your MongoDB instance.

3. **Install Required Packages**
   - Run the following commands in your project directory:
   ```bash
   composer require jenssegers/mongodb
   composer require mongodb/laravel-mongodb

4. **Set MongoDB Connection in `config/database.php`**
    - Add the following configuration in the database connections array:
    ```bash
    'mongodb' => [
            'driver' => 'mongodb',
            'dsn' => env('MONGODB_URI'),
            'database' => env('MONGODB_DATABASE'),
        ],
    ```
    
5. **set environment variables** 
    ```bash      
        MONGODB_URI=mongodb://localhost:27017/
        MONGODB_DB_NAME=AuditDataBase
    ```

6. **create a model for mongoAudit** 
    ```bash

        namespace Botble\Base\Models;

        use Jenssegers\Mongodb\Eloquent\Model;

        class MongoAudit extends Model implements \OwenIt\Auditing\Contracts\Audit
        {
            use \OwenIt\Auditing\Audit;

            /**
            * {@inheritdoc}
            */
            protected $guarded = [];

            /**
            * {@inheritdoc}
            */
            protected $casts = [
                'old_values'   => 'json',
                'new_values'   => 'json',
            ];

            /**
            * {@inheritdoc}
            */
            public function auditable()
            {
                return $this->morphTo();
            }

            /**
            * {@inheritdoc}
            */
            public function user()
            {
                return $this->morphTo();
            }
        }
    ```

7. **update config/audit**
    - set 

    ```bash
    'implementation' => MongoAudit::class,
    ```
    ```bash
      'drivers' => [
        'database' => [
            'table'      => 'audits',
            'connection' => 'mongodb',
        ],
    ],
    ```

