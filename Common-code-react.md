### Quy định đặt tên.

##### Tên folder: In thường

Ví dụ: components, api, container, pages, templates,...

##### Tên file .ts,.tsx, .js,.jsx,...

Ví dụ: `NoteEditor.tsx`, `NoteList.tsx`,
Riêng `index.tsx` viết thường

##### Tên file .ts

Ví dụ: `constants.ts`, `enums.ts`,`type.ts`

### Cú pháp mẫu fuction component

```javascript
interface CategoryOptionProps {
category: CategoryItem
  index: number
  contextMenuRef: React.RefObject<HTMLDivElement>
  handleCategoryMenuClick: (
    event: React.MouseEvent<HTMLDivElement, MouseEvent> | ReactMouseEvent,
    categoryId?: string
  ) => void
  handleCategoryRightClick: (
    event: React.MouseEvent<HTMLDivElement, MouseEvent> | ReactMouseEvent,
    categoryId?: string
  ) => void
  onSubmitUpdateCategory: (event: ReactSubmitEvent) => void
  optionsPosition: { x: number; y: number }
  optionsId: string
  setOptionsId: React.Dispatch<React.SetStateAction<string>>
}
export const CategoryOption: React.FC<CategoryOptionProps> = ({
  category,
  index,
  contextMenuRef,
  handleCategoryMenuClick,
  handleCategoryRightClick,
  onSubmitUpdateCategory,
  optionsPosition,
  optionsId,
  setOptionsId,
}) =>
{
    // ===========================================================================
    // Selectors
    // ===========================================================================

  const { codeMirrorOptions, isOpen, previewMarkdown, darkTheme, notesSortKey } =
    useSelector(getSettings)
  const { currentUser } = useSelector(getAuth)
  const { notes, activeFolder, activeCategoryId } = useSelector(getNotes)
  const { categories } = useSelector(getCategories)
   //           ===================================================================== ======
    // Dispatch
    // ===========================================================================
  const dispatch = useDispatch()

  const _logout = () => dispatch(logout())
  const _toggleSettingsModal = () => dispatch(toggleSettingsModal())
  const _togglePreviewMarkdown = () => dispatch(togglePreviewMarkdown())
  const _toggleDarkTheme = () => dispatch(toggleDarkTheme())

  // ===========================================================================
  // Refs
  // ===========================================================================

  const node = useRef<HTMLDivElement>(null)

  // ===========================================================================
  // Handlers
  // ===========================================================================

  const handleDomClick = (event: ReactMouseEvent) => {
    event.stopPropagation()

    if (node.current && node.current.contains(event.target as HTMLDivElement)) return
    if (isOpen) {
      _toggleSettingsModal()
    }
  }

  const togglePreviewMarkdownHandler = () => _togglePreviewMarkdown()
  const toggleDarkThemeHandler = () => {
    _toggleDarkTheme()
    _updateCodeMirrorOption('theme', darkTheme ? 'base16-light' : 'new-moon')
  }
// ===========================================================================
  // State ( React Context API)
  // ===========================================================================

  const { addingTempCategory, setAddingTempCategory } = useTempState()
  // ===========================================================================
  // Hooks
  // ===========================================================================

  useEffect(() => {
    document.addEventListener('mousedown', handleDomClick)
    document.addEventListener('keydown', handleEscPress)

    return () => {
      document.removeEventListener('mousedown', handleDomClick)
      document.removeEventListener('keydown', handleEscPress)
    }
  })

  return (
    <div>
    {categories && categories.length > 0 && categories.map((category, index) => (
        <CategoryOption
        key={category.id}
        index={index}
        category={category}
        contextMenuRef={contextMenuRef}
        handleCategoryMenuClick={handleCategoryMenuClick}
        handleCategoryRightClick={handleCategoryRightClick}
        onSubmitUpdateCategory={onSubmitUpdateCategory}
        optionsId={optionsId}
        setOptionsId={setOptionsId}
        optionsPosition={optionsPosition}
        />
    ))}
    </div>
  )
```

#### Khai báo đường dẫn tuyệt đối trong file tsconfig.json

```javascript
"paths": {
      // Allow `@/` to map to `src/client/`
      "@/*": ["./src/client/*"],
      "@resources/*": ["./src/resources/*"]
    }
```

#### Sử dụng Redux toolkit

Tạo foler `stores`
Trong folder slices tạo auth.ts

```js
...src>stores>auth.store.ts

import { createSlice, PayloadAction } from '@reduxjs/toolkit'

import { AuthState } from '@/types'

export const initialState: AuthState = {
  currentUser: {},
  isAuthenticated: false,
  error: '',
  loading: true,
}

const authSlice = createSlice({
  name: 'auth',
  initialState,
  reducers: {
    login: (state) => {
      state.loading = true
    },

    loginSuccess: (state, { payload }: PayloadAction<any>) => {
      state.currentUser = payload
      state.isAuthenticated = true
      state.loading = false
    },

    loginError: (state, { payload }: PayloadAction<string>) => {
      state.error = payload
      state.isAuthenticated = false
      state.loading = false
    },

    logout: (state) => {
      state.loading = true
    },

    logoutSuccess: (state) => {
      state.isAuthenticated = false
      state.currentUser = {}
      state.error = ''
      state.loading = false
    },
  },
})

export const { login, loginSuccess, loginError, logout, logoutSuccess } = authSlice.actions

export default authSlice.reducer

```

Tạo file rootReducer.tsx để tổng hợp các slice

```typescript
... >src>stores>rootReducer.tsx

import { combineReducers, Reducer } from 'redux'

import authReducer from '@/stores/auth.store'
import categoryReducer from '@/stores/category.store'
...
export interface RootState { // Nên để trong file riêng thư mục types
  authState: AuthState
  categoryState: CategoryState
}
const rootReducer: Reducer<RootState> = combineReducers<RootState>({
  authState: authReducer,
  categoryState: categoryReducer,
...
})

export default rootReducer

```

Tạo file index.ts để tạo configure store

```typescript
... >src>stores>index.ts

import { configureStore } from '@reduxjs/toolkit';
import rootReducer from './rootReducer';

const store = configureStore({
  reducer: rootReducer,
});

export type AppState = ReturnType<typeof rootReducer>;
export type AppDispatch = typeof store.dispatch;

export default store;

export type AppStore = typeof store;

```

Vào thư index.tsx của project để thêm Config store.

```typescript
import store from "./stores";
import { Provider } from "react-redux";

ReactDOM.render(
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById("root")
);
```

Tạo folder selectors

```js
... > selectors>index.ts
import { RootState } from '@/types'
export const getAuth = (state: RootState) => state.authState
```

Tạo các components sử dụng redux

```js
import { getAuth } from "@/selectors";
import { login } from "@/stores/auth";

// ===========================================================================
// Selectors
// ===========================================================================

const { loading } = useSelector(getAuth);

// ===========================================================================
// Dispatch
// ===========================================================================

const dispatch = useDispatch();

const _login = () => dispatch(login());
```

**Kiến thức bổ sung**:
`Mutable`: Là khái niệm 1 biến có thể thay đổi trực tiếp giá trị mà k cần phải tạo ra 1 bản copy khác.
`Immutable`: Ngược lại,không thể thay đổi object sau khi tạo ra, bắt buộc phải tạo ra 1 bản copy rồi mới thay đổi được.
`Tại sao nên dùng Redux toolkit thay vời Redux`: Vì Redux toolkit giúp giảm thiểu code, giảm thiểu cấu hình. Ví dụ trong redux cần phải tạo ra action types, action creators, reducers riêng trong khi khi RTK chỉ cần dùng createSlice là nó sẽ tự động tạo ra cả 3. Redux dùng cú pháp 'imumtable' trong khi đó RTK code dùng 'mutable' nhưng vẫn giữ được tính chất 'immutable'

### Router

```js
... >src>router>PrivateRoute.tsx

import React from 'react'
import { useSelector } from 'react-redux'
import { Redirect, Route, RouteProps } from 'react-router-dom'

import { getAuth } from '@/selectors'

interface PrivateRouteProps extends RouteProps {
  component: any
}

export const PrivateRoute: React.FC<PrivateRouteProps> = ({ component: Component, ...rest }) => {
  const { isAuthenticated } = useSelector(getAuth)

  return (
    <Route
      render={(props) =>
        isAuthenticated === true ? <Component {...props} /> : <Redirect to="/" />
      }
      {...rest}
    />
  )
}
```

```js
... >src>router>PublicRoute.tsx
import React from 'react'
import { useSelector } from 'react-redux'
import { Redirect, Route, RouteProps } from 'react-router-dom'

import { getAuth } from '@/selectors'

interface PublicRouteProps extends RouteProps {
  component: any
}

export const PublicRoute: React.FC<PublicRouteProps> = ({ component: Component, ...rest }) => {
  const { isAuthenticated } = useSelector(getAuth)

  return (
    <Route
      render={(props) =>
        isAuthenticated === false ? <Component {...props} /> : <Redirect to="/app" />
      }
      {...rest}
    />
  )
}
```

### Sử dụng Fragment để wrapper bên ngoài

Vấn đề: Khi trả về nhiều JSX tag - phải có 1 thẻ wrapper bên ngoài ( thường là thẻ dev, span,p).
Hậu quả: phá vỡ cấu trúc HTML, CSS bị phá vỡ cấu trúc => Phát sinh thêm nhiều wrapper tag
Từ phiên bản React ^16.2.0 có thể sử dụng cách sau
Nếu khôn muốn thêm 1 thẻ tag để wrapper toàn bộ các components, ta cần trả về 1 mảng các thẻ, cách nhau 1 dấu phảy, mỗi phần tử phải có 1 key. Nếu là chuỗi cần có ngoặc kép

```js
const App extends Component {
render(){
        [<div key="1">
            Learn React Native
        </div>,
        <div  key="2">
            Learn Angular
        </div>,
        <div  key="3">
            Learn Nextjs
        </div>]
    }
}
```

Sử dụng Fragment

```js
const App extends Component {
render(){
    <React.Fragment>
        <div>
            Learn React Native
        </div>,
        <div>
            Learn Angular
        </div>,
        <div >
            Learn Nextjs
        </div>
    </React.Fragment>
    }
}
```

### Search multiple objects filter

Bài toán: Tạo 1 ô search, ô search này sẽ tìm kiếm theo nhiều trường dữ liệu. Ví dụ tìm kiếm theo
![Bìa toán](https://i.imgur.com/2LVzStN.png) )