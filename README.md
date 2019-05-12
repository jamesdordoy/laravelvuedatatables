
# Laravel Vue Datatable
A Vue.js datatable component for Laravel that works with Bootstrap.

## Requirements

* [Vue.js](https://vuejs.org/) 2.x
* [Laravel](http://laravel.com/docs/) 5.x
* [Bootstrap](http://getbootstrap.com/) 4 (Optional)

This package makes use of an optional default component, the [Laravel Vue Pagination](https://github.com/gilbitron/laravel-vue-pagination)  component created by [gilbitron](https://github.com/gilbitron). If you need a pagination component for other areas of your website and you are using a Laravel API &amp; Bootstrap, i highly suggest using this flexible component.

## Demo

See [https://jamesdordoy.github.io/vue-datatable/](https://jamesdordoy.github.io/vue-datatable/)

## Table of Contents
- [Example](#example)
- [Package Installation](#package-installation)
	- [Add Service Provider](#add-service-provider)
  	- [Publish the Config](#job-portal)
  		- [Options](#community)
  	- [Use the Trait](#job-portal)
	- [Use the Controller Resource](#conferences)
- [Component Installation](#podcasts)
	- [Register the Plugin](#youtube-channels)
	- [Basic Example](#official-examples)
- [API](#tutorials)
	- [Datatable Props](#datatable-props)
- [Books](#books)
- [Blog Posts](#blog-posts)
- [Projects Using Vue.js](#projects-using-vuejs)

## Example
![Image description](https://www.jamesdordoy.co.uk/images/projects/bootstrap-datatable.png)

## Package Installation
```
$ composer require jamesdordoy/laravelvuedatatable
```

### Add Service Provider
```
JamesDordoy\LaravelVueDatatable\Providers\LaravelVueDatatableServiceProvider::class,
```

### Publish the Config
```
$ php artisan vendor:publish --provider="JamesDordoy\LaravelVueDatatable\Providers\LaravelVueDatatableServiceProvider"
```

#### Options

```json
{
    "models": {
        "search_term": "The term used in each model to declare if it is searchable by the datatable"
    },
    "default_order_by": "the default order by column on page loads"
}
```


### Use the Trait
This trait is optional and simply provides a basic method for filtering your data based on the $dataTableColumns attribute set in the model. If you would like more control on how the data is filtered, feel free to omit this trait use your own filtering methods. Just remember to paginate the results!

```php
<?php

namespace App;

use Illuminate\Notifications\Notifiable;
use Illuminate\Contracts\Auth\MustVerifyEmail;
use Illuminate\Foundation\Auth\User as Authenticatable;
use JamesDordoy\LaravelVueDatatable\Traits\LaravelVueDatatableTrait;

class User extends Authenticatable
{
    use Notifiable, LaravelVueDatatableTrait;

    protected $dataTableColumns = [
        'id' => [
            'searchable' => false,
        ],
        'name' => [
            'searchable' => true,
        ],
        'email' => [
            'searchable' => true,
        ]
    ];
}
```

### Use the Controller Resource
The Collection Resource is expecting a paginated collection, so feel free to use your own queries if your require more complex filtering.

```php
<?php

namespace App\Http\Controllers\Back;

use App\User;
use Illuminate\Http\Request;
use App\Http\Controllers\Controller;
use JamesDordoy\LaravelVueDatatable\Http\Resources\DataTableCollectionResource;

class UserController extends Controller
{
    public function index(Request $request)
    {   
        $length = $request->input('length');
        $column = $request->input('column'); //Index
        $dir = $request->input('dir', 'asc');
        $searchValue = $request->input('search');

        $data = User::dataTableQuery($column, $dir, $length, $searchValue);

        return new DataTableCollectionResource($data);
    }
}
```

## Component Installation

```bash
npm install laravel-vue-datatable
```

### Register the Plugin

```javascript
import DataTable from 'laravel-vue-datatable';
Vue.use(DataTable);
```

### Basic Example 

```html
<data-table
    url="http://vue-datatable.test/ajax"
    :per-page="perPage"
    :columns="columns">
</data-table>
```

```javascript
export default {
    name: 'app',
    data() {
        return {
            perPage: ['10', '25', '50'],
            columns: [
                {
                    label: 'ID',
                    name: 'id',
                    filterable: true,
                },
                {
                    label: 'Name',
                    name: 'name',
                    filterable: true,
                },
                {
                    label: 'Email',
                    name: 'email',
                    filterable: true,
                },
                {
                    label: '',
                    name: 'View',
                    filterable: false,
                },
            ]
        }
    },
}
```

## API

### Datatable Props

| Name | Type | Default | Description  
| --- | --- | --- | --- |
| `url ` | String | "/" | The JSON url |
| `columns` | Array | [] | The table columns |
| `per-page` | Array | [ '10', '25', '50' ] | (optional) Amount to be displayed |
| `classes` | Object | See Below | (optional) Table classes |
| `pagination` | Object | {}  | (optional) props for [gilbitron/laravel-vue-pagination](https://github.com/gilbitron/laravel-vue-pagination#props) |

### Default Classes

```json
{
    "table-container": {
        "table-responsive": true,
    },
    "table": {
        "table": true,
        "table-striped": true,
        "table-dark": true,
    },
    "t-head": {

    },
    "t-body": {
        
    },
    "t-head-tr": {

    },
    "t-body-tr": {
        
    },
    "td": {

    },
    "th": {
        
    },
}
```

### Column Props
| Name | Type | Default | Description  
| --- | --- | --- | --- |
| `label ` | String | "" | The JSON url |
| `name` | String | "" | The table column header name |
| `width` | Number | 0 | The table column width |
| `filterable` | Boolean | false | Is the column filterable |
| `component` | Component | null | A dynamic component that can be injected |
| `classes` | Object | {} | Component classes to parse |


## Using dynamic components
You can also inject your own components into the table such as buttons. Your buttons, links etc can also listen for events.

### Example button component (ExampleButton.vue)

```html
<template>
    <button :class="classes" @click="click(data)" title="Update">
        <span>
            <i class="fa fa-eye" aria-hidden="true"></i>
        </span>
        &nbsp;
        {{ name }}
    </button>
</template>
```

```javascript
export default {
    props: {
        data: {},
        name: {},
        click: {},
        classes: {},
    }
}
```

### Datatable Columns (UserDataTable.vue)
```javascript

import ExampleButton './ExampleButton.vue';

export default {
    data() {
        return {
            url: 'http://vue-datatable.test/ajax',
            perPage: ['10', '25', '50'],
            columns: [
            {
                label: 'ID',
                name: 'id',
                filterable: true,
            },
            {
                label: 'Name',
                 name: 'name',
                filterable: true,
            },
            {
                label: 'Email',
                name: 'email',
                filterable: true,
            }
            {
                label: '',
                name: 'View',
                filterable: false,
                component: ExampleButton,
                click: this.alertMe,
                classes: { 
                    'btn': true,
                    'btn-primary': true,
                    'btn-sm': true,
                } 
            },
            ]
        }
    },
    components: {
        // eslint-disable-next-line
        ExampleButton,
    },
    methods: {
        alertMe(data) {
            alert("hey");
        }
    },
}
```

## Overriding the Filters &amp; Pagination Components
If the included pagination or filters do not meet your requirements or if the styling is incorrect, they can be over-written using scoped slots.

### DataTable

```html
<data-table
    :url="url"
    :columns="columns"
    :per-page="perPage">

    <span slot="pagination" slot-scope="{ links, meta }">
        <paginator 
            :meta="meta"
            :links="links"
            @next="updateUrl"
            @prev="updateUrl">
        </paginator>
    </span>
</data-table>
```

Once the URL has been updated by your customer paginator or filters, the table will re-render. Alterativly, if updating the URL is troublesome, different table filters can be manipulated by your filters using the v-model directive:

### Example filter (DataTableFilter.vue)
This example filter will control the length of the table manipulating the tableData.length property using v-model.

```html
<template>
    <select
        class="form-control"
        v-model="tableData.length">
        <option
            :key="index"
            :value="records"
            v-for="(records, index) in perPage">
            {{ records }}
        </option>
    </select>
</template>
```

```javascript
export default {
    props: [
        "tableData", "perPage"
    ]
}
```

### DataTable

```html
<data-table
    :url="url"
    :columns="columns"
    :per-page="perPage">

    <span slot="filters" slot-scope="{ tableData, perPage }">
        <data-table-filters
            :per-page="perPage"
            :table-data="tableData">
        </data-table-filters>
    </span>
</data-table>
```

## Styling the Datatable
You can edit the style of the datatable by overriding the classes prop. A example mixin config can be found be below for Tailwind:

![Image description](https://www.jamesdordoy.co.uk/images/projects/tailwind-datatable.png)

### Tailwind Config (mixins/tailwind.js)

Custom Class

```css
  .stripped-table:nth-child(even) {
    @apply bg-black;
  }
```

```javascript
export default {
    data() {
        return {
            classes: { 
                'table-container': {
                    'justify-center': true,
                    'w-full': true,
                    'flex': true,
                    'rounded': true,
                    'mb-6': true,
                    'shadow-md': true,
                },
                table: {
                    'text-left': true,
                    'w-full': true,
                    'border-collapse': true,
                },
                't-head': {
                    'text-grey-dark': true,
                    'bg-black': true,
                    'border-grey-light': true,
                    'py-4': true,
                    'px-6': true,
                },
                "t-body": {
                    'bg-grey-darkest': true,
                    
                },
                "t-head-tr": {
                    
                },
                "t-body-tr": {
                    'stripped-table': true,
                    'bg-grey-darkest': true,
                },
                "td": {
                    'py-4': true,
                    'px-6': true,
                    'border-b': true,
                    'border-grey-light': true,
                    'text-grey-light': true,
                },
                "th": {
                    'py-4': true,
                    'px-6': true,
                    'font-bold': true,
                    'uppercase': true,
                    'text-sm': true,
                    'text-grey-dark': true,
                    'border-b': true,
                    'border-grey-light': true,
                },
            }
        };
    },
}
```

### Datatable

```html
<data-table
    :url="url"
    :columns="columns"
    :classes="classes"
    :per-page="perPage">
    <span slot="filters" slot-scope="{ tableData, perPage }">
        <data-table-filters
            :table-data="tableData"
            :per-page="perPage">
        </data-table-filters>
    </span>

    <span slot="pagination" slot-scope="{ links, meta }">
        <paginator 
            @next="updateUrl"
            @prev="updateUrl"
            :meta="meta"
            :links="links">
        </paginator>
    </span>
</data-table>
```

```javascript

import TailwindDatatable from '../mixins/tailwind.js';

export default {
    data() {
        return {
            url: 'http://vue-datatable.test/ajax',
            perPage: ['10', '25', '50'],
            columns: [
            {
                label: 'ID',
                name: 'id',
                filterable: true,
            },
            {
                label: 'Name',
                 name: 'name',
                filterable: true,
            },
            {
                label: 'Email',
                name: 'email',
                filterable: true,
            }
            ]
        }
    },
	mixins: [
		TailwindDatatable
	]
}
```