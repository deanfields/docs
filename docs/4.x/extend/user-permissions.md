# User Permissions

Modules and plugins can register new [user permissions](../user-management.md#permissions) to the system using the [EVENT_REGISTER_PERMISSIONS](craft4:craft\services\UserPermissions::EVENT_REGISTER_PERMISSIONS) event:

```php
use craft\events\RegisterUserPermissionsEvent;
use craft\services\UserPermissions;
use yii\base\Event;

public function init()
{
    parent::init();

    Event::on(
        UserPermissions::class,
        UserPermissions::EVENT_REGISTER_PERMISSIONS,
        function(RegisterUserPermissionsEvent $event) {
            $event->permissions['Permission Group Name'] = [
                'permissionName' => [
                    'label' => 'Permission Label',
                ],
            ];
        }
    );
}
```

Permissions can also have nested permissions by adding a `nested` key to the permission array.

```php
'permissionName' => [
    'label' => 'Permission Label',
    'nested' => [
        'nestedPermissionName' => [
            'label' => 'Nested Permission Label',
        ],
    ],
];
```

::: tip
Nesting is meant for UI only; if you wanted to reference `nestedPermissionName` in the example above you would use exactly that key.
:::

## Requiring Permissions

Controllers can require that the logged-in user has a permission by calling [requirePermission()](craft4:craft\web\Controller::requirePermission()).

```php
public function actionStayUpLate()
{
    // Require the `stayUpLate` permission
    $this->requirePermission('stayUpLate');
}
```

If the user doesn’t have that permission, then a 403 error will be returned.

Templates can also ensure that the user has a permission with the [requirePermission](../dev/tags.md#requirepermission) tag:

```twig
{% requirePermission 'stayUpLate' %}
```

## Checking Permissions

You can check if the logged-in user has a permission by calling <craft4:craft\web\User::checkPermission()>:

```php
// See if they have the `stayUpLate` permission
if (Craft::$app->user->checkPermission('stayUpLate')) {
    // ...
}
```

You can also see if any given user has a permission by calling <craft4:craft\elements\User::can()>:

```php
/** @var \craft\elements\User $user */
if ($user->can('stayUpLate')) {
    // ...
}
```
