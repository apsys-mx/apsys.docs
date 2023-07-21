# Creación de un Indice

Si creaste tu plantilla, a partir del repositorio `apsys-mx/apsys.frontend.base.turkey`, asegúrate de tener estas carpetas para poder generar el index.

- common
- dataGrid
- filters

![Untitled](Resources/common.png)

Abre la consola de visual:

![Untitled](Resources/console.png)

Instala los siguientes paquetes que contendrán lo requerido para el funcionamiento del Índice.

- Install moment

```ruby linenums="1"
npm install moment
```

- Install uuidv4

```ruby linenums="1"
npm install uuidv4
```

- Install react-select

```ruby linenums="1"
npm install react-select
```

- Install react-date-range

```ruby linenums="1"
npm install react-date-range
```

#### **Archivo endpoint**

Conecta el backend con el frontend, este proceso es diferente de acuerdo con el proyecto.

Para comprobar si el back está conectado con el front entra a la carpeta `store` y ahí debe de haber un archivo donde se configura la URL donde nos vamos a conectar al back.

- ![Untitled](Resources/urlBack.png)

Comprobamos si la URL corresponde con la del back:

- ![Untitled](Resources/urlFront.png)

## Configuración del api Slice

Para obtener la información del index se debe generar un endpoint para realizar la petición de back.

Si no existe el archivo donde se configuran los endpoints, crea uno para comenzar a crear los métodos donde vamos a obtener la información del back y los catálogos para los filtros.

Creada a partir del módulo a trabajar, para este ejemplo seria: => `home.endPoints.js`

- ![Untitled](Resources/endPoint.png)

Estructura del endpoint para obtener la información aplicando paginado, filtros y ordenamiento.

- Especificamos por la columna se va a ordenar la información. En el ejemplo indicamos => `CreationDate`

```ruby linenums="1"
import { timesheetsApi } from '../../store/timesheet-api'
export const timesheetsEndPoint = timesheetsApi.injectEndpoints({
    endpoints: (builder) => ({
        getTimesheets: builder.query({
            query(params) {
                const { sorting, pagination, filters } = params
                var { sortBy, sortDirection } = sorting
                var { pageNumber, pageSize } = pagination
                pageNumber = pageNumber ? pageNumber : 0
                pageSize = pageSize ? pageSize : 0
                sortDirection = sortDirection && sortDirection.length > 0 ? sortDirection : 'desc'
                sortBy = sortBy && sortBy.length > 0 ? sortBy : 'projectName'
                var url = `Timesheets?sortBy=${sortBy}&sortDirection=${sortDirection}&pageNumber=${pageNumber}&pageSize=${pageSize}&${filters}`
                return {
                    url: url,
                    method: 'GET',
                }
            },
        }),

    }),
    overrideExisting: true,
})
export const { useGetTimesheetsQuery, useGetCatalogsQuery } = timesheetsEndPoint
```

Adicional se agrega un endpoint para obtener la lista de catálogo para aplicar en los filtros con el nombre `getCatalogs`:

```ruby linenums="1"
getCatalogs: builder.query({
			query(fieldName) {
				return {
					url: `Timesheets/catalogs/${fieldName}`,
					method: 'GET',
				}
			},
		}),
```

#### **Archivo Slice **

-El achivo Slice es aquel donde se encuentra la configuración del initialState: La cual contiene paginación, ordenamiento y filtros.

- El listado de reducers: Para la actualización de initialState.

Si no existe el archivo slice, crea uno para configurar el initialState.

- ![Untitled](Resources/slice.png)

-En caso de que exista agregar los métodos faltantes.

```ruby linenums="1"

import { createSlice } from '@reduxjs/toolkit'

const defaultToasterState = {
	open: false,
	message: '',
	severity: 'info',
}

const initialState = {

	pagination: {
		rowsCount: 0,
		rowsPerPage: 20,
		page: 0,
	},
	sorting: {
		sortBy: '',
		sortDirection: '',
	},

	filters: '',
}


export const homeSlice = createSlice({
	name: 'homeSlice',
	initialState,
	reducers: {
		setPageNumber: (state, action) => {
			state.pagination.page = action.payload
		},
		setPageSize: (state, action) => {
			state.pagination.rowsPerPage = action.payload
		},
		setSorting: (state, action) => {
			state.sorting.sortBy = action.payload.sortBy
			state.sorting.sortDirection = action.payload.sortDirection
		},
		setFilter: (state, action) => {
			state.filters = action.payload
		},

	},
})

export const {
	setTitle,
	setToasterState,
	setPageNumber,
	setPageSize,
	setSorting,
	setFilter,

} = homeSlice.actions
export default homeSlice.reducer
```

#### **Archivo Selectors **

- Archivos que contenga los Selectors: Donde obtendremos la información initialState

- Si no existe el archivo selectors, crea uno y agrega los métodos faltantes

- ![Untitled](Resources/selectors.png)

```ruby linenums="1"
import { createSelector } from 'reselect'

const getState = (state) => (state = state.homeSlice)


const getViewState = createSelector(getState, (state) => {
	return state
})

const getPagination = createSelector(getViewState, (state) => {
	return state.pagination
})
const getSorting = createSelector(getViewState, (state) => {
	return state.sorting
})

const getFilters = createSelector(getViewState, (state) => {
	return state.filters
})

export {  getPagination, getSorting, getFilters, getViewState }
```

### **Creación de la tabla index **

Para generar la tabla, es necesario revisar si contamos con los siguientes archivos y que contenga la siguiente información:

### **Crear configuración de tabla**

Crear el archivo 'configurationTable.jsx'

- ![Untitled](Resources/configurationTable.png)

Este archivo contiene la confirmación de la tabla, aquí indicamos las columnas que queremos que aparecen en la tabla:
|

```ruby linenums="1"
export const localTableConfig = [
    {
       title: 'Codigo del proyecto', //=>Nombre de la columna
		sortable: true, //=>Si aplicará ordenmiento en la columna
		dataSource: 'projectCode', //=>Nombre que regrea el back
		isActiveFilter: true, //=>Si aplicará filtro en la columna
		filterType: 'text', //=>>El tipo de filtro
    },
        {
        title: 'Fecha de inicio',
        sortable: true,
        dataSource: 'startDate',
        isActiveFilter: true,
        filterType: 'date',
    },
        {
        title: 'Duración',
        sortable: true,
        dataSource: 'duration',
        isActiveFilter: true,
        filterType: 'numeric',
    },
]

```

#### **Implementar DataGrid **

Genera un nuevo archivo o buscamos el archivo donde queremos implementar la tabla:

![Untitled](Resources/home.png)

En este documento importarás la configuración anterior de la tabla, así como el archivo de datagrid y el paginado ya configurado en el proyecto base.

![Untitled](Resources/import.png)

- Es necesario llamar los selectors para obtener el paginado, ordenamiento y filtros:

```ruby linenums="1"
import * as selectors from '../home.selectors'
```

```ruby linenums="1"
	const viewPaginationState = useSelector((state) => selectors.getPagination(state))
	const viewSortingState = useSelector((state) => selectors.getSorting(state))
	const viewFilter = useSelector((state) => selectors.getFilters(state))
```

- Llamar el endpoint que realizamos para obtener la información del back mandando la información que obtenemos de los selectores del initialState.
- Este endpoint nos regresará un isLoading, isError y una data que contendrá la lista de Items, total, pageSize y pageNumber.

```ruby linenums="1"
const {
		data: timeSheetsResponse,
		isLoading,
		isError,
		error,
	} = useGetTimesheetsQuery({
		pagination: {
			pageNumber: viewPaginationState.page,
			pageSize: viewPaginationState.rowsPerPage,
		},
		sorting: {
			sortBy: viewSortingState.sortBy,
			sortDirection: viewSortingState.sortDirection,
		},
		filters: viewFilter ? viewFilter : '',
	})
```

Podemos confirmar que recibimos información de back mandando a consola él data y podemos ver qué información revivimos en el navegador:

![Untitled](Resources/ConsoleLogData.png)

La lista de item podemos ver la información que recibimos y aquí podemos determinar que información queremos que aparezca y podemos agregar los campos en la configuración de la tabla:

![Untitled](Resources/BrowserConsole.png)

Generaremos métodos para llamar las acciones que permitan modificar el paginado y ordenamiento:

```ruby linenums="1"
 	const handleChangePage = (pageNumber) => {
		dispatch(setPageNumber(pageNumber))
	}
	const handleChangeRowsPerPage = (pageSize) => {
		dispatch(setPageSize(pageSize))
	}
	//:::::::::::::::::::::::::::::::::::::::::::::::::::://
	//:::::::::::::::::(Sorting):::::::::::::::::::::::::://
	const onchangeSorting = (sort, direction) => {
		dispatch(setSorting({ sortBy: sort, sortDirection: direction }))
	}
	//:::::::::::::::::::::::::::::::::::::::::::::::::::://
```

Si quieres realizar alguna modificación de las columnas puedes realizar la siguiente configuración:

- Se pasa la configuración de la tabla, tableConfig.

- Se indica la tabla que quieres modificar y el cambio que quieras realizar

````ruby linenums="1"
const [localTableConfig, setLocalTableConfig] = useState([])
	useEffect(() => {
		if (tableConfig) {
			var local = tableConfig.map((config) => {
				return { ...config }
			})
			setLocalTableConfig(local)
		}
	}, [tableConfig])
	/**
	 * Get the header configuration
	 */
	const enhancedConfiguration = localTableConfig.map((config) => {
		switch (config.dataSource) {//indicamos la columna que queremos modificar
			case 'startDate':
				config.onRenderProperty = (item) => {
					return moment(item.startDate).format('DD/MM/YYYY')//El cambio que queremos realizar
				}
				break
			case 'endDate':
				config.onRenderProperty = (item) => {
					return moment(item.endDate).format('DD/MM/YYYY')
				}
				break
			default:
		}
		return config
	})
    ```
````

Ahora llamamos la tabla y le pasamos los valores correspondientes:
-Pasamos la configuración de la tabla
-la lista de item que recibimos del data
-Agregamos el método de ordenamiento y paginados

```ruby linenums="1"
	<Box>

			<DataGrid
				headers={enhancedConfiguration}
				data={data.items}
				onchangeSorting={onchangeSorting}
				sortBy={sortBy}
				sortDirection={sortDirection}
			/>
			<Pagination
				pagination={data}
				onPageChange={handleChangePage}
				onRowsPerPageChange={handleChangeRowsPerPage}
			/>
		</Box>

```

![Untitled](Resources/index.png)

![Untitled](Resources/filters.png)
