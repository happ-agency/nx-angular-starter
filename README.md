# [Nx](https://nx.dev/) [Angular](https://angular.io/) Starter

> Based on the [happ/nx-starter](https://github.com/happ-agency/nx-starter)

## Content

1. [Nrwl angular](#nrwl-angular)
2. [Nrwl angular app](#nrwl-angular-app)
3. [Nrwl angular library](#nrwl-angular-library)
4. [Nrwl angular publishable library](#nrwl-angular-publishable-library)
5. [Angular localize](#angular-localize)
6. [Ngx i18n support tooling](#ngx-i18n-support-tooling)
7. [Xliffmerge autotranslate feature](#xliffmerge-autotranslate-feature)
8. [Angular universal](#angular-universal)
9. [Angular hmr](#angular-hmr)
10. [Css reset](#css-reset)
11. [Normalize](#normalize)
12. [Angular material](#angular-material)
13. [Tailwind](#tailwind)
14. [Angular flex layout](#angular-flex-layout)
15. [Structure](#structure)
16. [Used libraries](#used-libraries)
17. [Nice to use libraries](#nice-to-use-libraries)
18. [Related projects](#related-projects)

## Nrwl Angular
> [Nrwl angular](https://www.npmjs.com/package/@nrwl/angular)
1. Install [@nrwl/angular](https://www.npmjs.com/package/@nrwl/angular) only for dev: `pnpm i -D @nrwl/angular`

## Nrwl angular app
> [Nrwl angular application](https://nx.dev/latest/angular/angular/application)

1. Create an [Nrwl angular application](https://nx.dev/latest/angular/angular/application): `pnpx nx g @nrwl/angular:app APP_NAME --style=scss --routing=true`
	> 1. `APP_NAME` - name of the application
	> 2. `--style=scss` - default styles
	> 3. `--routing=true` - use routes by default or not
2. Create:
	1. `environment.example.ts` from `environment.ts`
	2. `environment.prod.example.ts` from `environment.prod.ts`
3. Update `.gitignore` with:
	```
	# APP_NAME Angular Environment
	/apps/APP_NAME/src/environments/environment.ts
	/apps/APP_NAME/src/environments/environment.prod.ts
 	```
 
	> Use `environment.*.example.ts` to push on git.

## Nrwl angular library
> [Nrwl angular library](https://nx.dev/latest/angular/angular/library)

1. Create an [Nrwl angular library](https://nx.dev/latest/angular/angular/library): `pnpx nx g @nrwl/angular:lib LIB_NAME`
	> 1. `APP_NAME` - name of the application

## Nrwl angular publishable library
> [Nrwl angular library](https://nx.dev/latest/angular/angular/library)

1. Create an [Nrwl angular publishable library](https://nx.dev/latest/angular/angular/library): `pnpx nx g @nrwl/angular:lib LIB_NAME --publishable --importPath=NPM_NAME`
	> 1. `APP_NAME` - name of the application
	> 2. `--publishable` - to create build target in the `workspace.json` which is the same
	> 3. `--importPath=NPM_NAME` - name of npm package

## Angular localize
> [Angular localize](https://angular.io/guide/i18n)

1. Install [@angular/localize](https://angular.io/guide/i18n) only for dev: `pnpm i -D @angular/localize`
2. Update `apps/app-name/src/polyfills.ts` with:
	 <details>
		 <summary>polyfills.ts</summary>

		import '@angular/localize/init';
	 	...
	 </details>
3. Update `workspace.json` with:

	 <details>
	 	<summary>workspace.json</summary>
		
		{
	 		...
			"projects" {
	 			...
	 			"APP_NAME": {
					...
					"i18n": {
						"sourceLocale": "LANGUAGE"
					},
					"targets": {
						"build": {
							...
							configurations: {
								...
								"LANGUAGE": {
									"aot": true,
									"outputPath": "dist/apps/APP_NAME_LANGUAGE",
									"i18nFile": "apps/APP_NAME/src/i18n/messages.LANGUAGE.xlf",
									"i18nFormat": "xlf",
									"i18nLocale": "LANGUAGE"
								}
							}
						}
						"serve": {
							configurations: {
								"LANGUAGE": {
									"browserTarget": "app-name:build:LANGUAGE"
								}
							}
						}
					}
	 			}
	 		}
	 	}
	 </details> 

4. Update your `package.json` with:

	 <details>
	 	<summary>package.json</summary>
	 
	 	{
	 		...
	 		"scripts": {
	 			...
				"extract-i18n": "nx extract-i18n --output-path apps/APP_NAME/src/i18n",
				"dev:ru": "nx serve --configuration=ru"
	 		}
	 	}

	 </details> 

## Ngx-i18n support tooling
> [Ngx-i18n support tooling](https://www.npmjs.com/package/@ngx-i18nsupport/tooling)

1. Install [@ngx-i18nsupport/tooling](https://www.npmjs.com/package/@ngx-i18nsupport/tooling) only for dev: `pnpm i -D @ngx-i18nsupport/tooling`
2. Update your `workspace.json` with:

	 <details>
		<summary>workspace.json</summary>

		{
	 		...
			"projects" {
	 			...
				"APP_NAME" {
	 				...
					"targets" {
						...
						"xliffmerge": {
							"executor": "@ngx-i18nsupport/tooling:xliffmerge",
							"options": {
								"xliffmergeOptions": {
									"i18nFormat": "xlf",
									"srcDir": "apps/APP_NAME/src/i18n",
									"genDir": "apps/APP_NAME/src/i18n",
									"defaultLanguage": "en",
									"languages": [
										"en",
										"ru"
									]
								}
							}
	 					}
					}
				}
			}
	 	}
	 </details> 
	 
3. Update your `package.json` with:

	 <details>
	 	<summary>package.json</summary>

	 	{
	  		...
	 		"scripts": {
	 			...
	 			"xliffmerge": "nx run app-name:xliffmerge"
	 		}
	 	}
	 </details> 
	 
## Xliffmerge autotranslate feature
> [xliffmerge-autotranslate-feature](https://github.com/martinroob/ngx-i18nsupport/wiki/xliffmerge-autotranslate-feature)

1. Update `workspace.json` with:

	<details>
		<summary>workspace.json</summary>
	 
		{
			...
			"projects" {
				...
				"APP_NAME" {
					...
					"targets" {
						...
						"xliffmerge": {
						 	...
							"autotranslate": true,
							"apikey": "GOOGLE_API_KEY"
						}
					}
				}
			}
		}
	</details> 


## Angular Universal
> [Angular Universal](https://angular.io/guide/universal)

1. Install [Angular Universal](https://angular.io/guide/universal): `pnpm i @nguniversal/express-engine`
2. Add [Angular Universal](https://angular.io/guide/universal) to your app: `pnpx nx g @nguniversal/express-engine:ng-add --client-project=APP_NAME`
	> 1. `APP_NAME` - name of the application
3. Replace `ng` to `nx` in `package.json`
4. Replace `dist/app-name` to `dist/apps/app-name` in `package.json`, `server.ts` and `workspace.json`

## Angular HMR
> [Angular HMR](https://habr.com/ru/company/skillfactory/blog/527516/)

1. Update `src/tsconfig.app.json` with

	<details>
		<summary>tsconfig.app.json</summary>
		
		"compilerOptions": {
	 		"types": ["node"]
		},
	</details> 

2. Update `src/main.ts` with:

	<details>
		<summary>main.ts</summary>

		import { enableProdMode } from '@angular/core';
		import { platformBrowserDynamic } from '@angular/platform-browser-dynamic';
		
		import { AppModule } from './app/app.module';
		import { environment } from './environments/environment';

		if (environment.production) {
			enableProdMode();
		}
	 
		function bootstrap() {
			platformBrowserDynamic()
				.bootstrapModule(AppModule)
				.catch((err) => console.error(err));
		}
		
		if (module['hot']) {
			bootstrap();
		} else {
			document.addEventListener('DOMContentLoaded', () => {
				bootstrap();
			});
		}
	</details>  

3. Update `package.json` with:

	<details>
		<summary>package.json</summary>

		{
			...
			"scripts": {
				...
				 "dev:hmr": "nx serve --hmr"
			}
			...
		}
 	</details>
	
## Css reset
> [Css reset](https://meyerweb.com/eric/tools/css/reset/)

1. Create `apps/APP_NAME/src/assets/styles/reset.css` with:
	 <details>
	 <summary>reset.css</summary>

		html, body, div, span, applet, object, iframe,
		h1, h2, h3, h4, h5, h6, p, blockquote, pre,
		a, abbr, acronym, address, big, cite, code,
		del, dfn, em, img, ins, kbd, q, s, samp,
		small, strike, strong, sub, sup, tt, var,
		b, u, i, center,
		dl, dt, dd, ol, ul, li,
		fieldset, form, label, legend,
		table, caption, tbody, tfoot, thead, tr, th, td,
		article, aside, canvas, details, embed,
		figure, figcaption, footer, header, hgroup,
		menu, nav, output, ruby, section, summary,
		time, mark, audio, video {
			margin: 0;
			padding: 0;
			border: 0;
			font-size: 100%;
			font: inherit;
			vertical-align: baseline;
		}
		article, aside, details, figcaption, figure,
		footer, header, hgroup, menu, nav, section {
			display: block;
		}
		body {
			line-height: 1;
		}
		ol, ul {
			list-style: none;
		}
		blockquote, q {
			quotes: none;
		}
		blockquote:before, blockquote:after,
		q:before, q:after {
			content: '';
			content: none;
		}
		table {
			border-collapse: collapse;
			border-spacing: 0;
		}
	  </details>
2. Update `styles.scss` with: 

	 <details>
	 	<summary>styles.scss</summary>
	 
		...
	 	@import '~assets/styles/reset.css';
	 </details>

## Normalize
> [Normalize](https://necolas.github.io/normalize.css/)

1. Install [normalize](https://necolas.github.io/normalize.css/) `pnpm i normalize.css`
2. Update `styles.scss` with:

	<details>
		<summary>styles.scss</summary>

		...
	 	@import '~normalize.css';
	</details>

## Angular material
> [Angular material](https://www.npmjs.com/package/@angular/material)

1. Install [@angular/material](https://www.npmjs.com/package/@angular/material): `pnpm i @angular/material`
2. Add [@angular/material](https://www.npmjs.com/package/@angular/material) to app: `pnpx nx g @angular/material:ng-add`

## Tailwind
> [Tailwind](https://www.npmjs.com/package/@ngneat/tailwind)

1. Install [@ngneat/tailwind](https://www.npmjs.com/package/@ngneat/tailwind): `pnpm i @ngneat/tailwind`
2. Add [@ngneat/tailwind](https://www.npmjs.com/package/@ngneat/tailwind) to app: `pnpx nx g @ngneat/tailwind:ng-add`
3. Install [official plugins](https://tailwindcss.com/docs/plugins): `pnpm i @tailwindcss/typography @tailwindcss/forms @tailwindcss/line-clamp @tailwindcss/aspect-ratio`
2. Update `tailwind.config.js` with:

	 <details>
	 	<summary>tailwind.config.js</summary>

		module.exports = {
			...
			plugins: [
				require('@tailwindcss/typography'),
				require('@tailwindcss/forms'),
				require('@tailwindcss/line-clamp'),
				require('@tailwindcss/aspect-ratio'),
			],
		}
	 </details>

> ! IMPORTANT
> 
> Setup [Tailwind](#tailwind) after the [Angular Material](#angular-material). 
> 
> `Angular Material` schematics will error out if it detects a custom webpack

## Angular flex layout
> [Angular flex layout](https://www.npmjs.com/package/@angular/flex-layout)

1. Install [@angular/flex-layout](https://www.npmjs.com/package/@angular/flex-layout): `pnpm i @angular/flex-layout @angular/cdk`
2. Add `FlexLayoutModule` to the module

## Structure

 ```
	┌─ apps/ 
	│  ┌─ APP_NAME/
	│  │  ┌─ src/
	│  │  │  ┌─ app/
	│  │  │  │  ┌─ core/
	│  │  │  │  │  ┌─ layout/
	│  │  │  │  │  │  ┌─ core.coponent.html
	│  │  │  │  │  │  ├─ core.component.scss
	│  │  │  │  │  │  ├─ core.component.spec.ts
	│  │  │  │  │  │  └─ core.component.ts
	│  │  │  │  │  ├─ core.routing.module.ts
	│  │  │  │  │  └─ core.module.ts
	│  │  │  │  ├─ shared/
	│  │  │  │  │  └─ shared.module.ts
	│  │  │  │  ├─ */
	│  │  │  │  │  ┌─ layout/
	│  │  │  │  │  │  ┌─ *.coponent.html
	│  │  │  │  │  │  ├─ *.component.scss
	│  │  │  │  │  │  ├─ *.component.spec.ts
	│  │  │  │  │  │  └─ *.component.ts
	│  │  │  │  │  ├─ *.routing.module.ts
	│  │  │  │  │  └─ *.module.ts
	│  │  │  │  └── app.module.ts
	│  │  │  ├── assets/
	│  │  │  │  ┌─ images/
	│  │  │  │  └─ styles/
	│  │  │  ├─ environment/
	│  │  │  │  ┌─ environment.*.ts
	│  │  │  │  └─ environment.*.example.ts
	│  │  │  ├── i18n/
	├──┴──┴──┘  └─ messages.*.xlf
	├─ libs/ 
	│  ┌─ LIB_NAME/
	│  │  ┌─ src/
	│  │  │  ┌─ lib/
	│  │  │  ├── LIB_NAME.module.ts
	└──┴──┴──┘
 ```

## Used Libraries
1. [@nrwl/angular](https://www.npmjs.com/package/@nrwl/angular)
2. [@angular/cli](https://www.npmjs.com/package/@angular/cli)
3. [@angular/localize](https://angular.io/guide/i18n)
4. [@ngx-i18nsupport/tooling](https://www.npmjs.com/package/@ngx-i18nsupport/tooling)
5. [@nguniversal/express-engine](https://www.npmjs.com/package/@nguniversal/express-engine)
6. [@angularclass/hmr](https://www.npmjs.com/package/@angularclass/hmr)
7. [@angular/material](https://www.npmjs.com/package/@angular/material)
8. [@ngneat/tailwind](https://www.npmjs.com/package/@ngneat/tailwind)
9. [@angular/flex-layout](https://www.npmjs.com/package/@angular/flex-layout)

## Nice to use libraries

1. [Advertising](#advertising)
2. [Animation](#animation)
3. [App tour](#app-tour)
4. [Auth](#auth)
5. [Avatar](#avatar)
6. [Backend](#backend)
7. [Calendars](#calendars)
8. [Carousel](#carousel)
9. [Charts](#charts)
10. [Config](#config)
11. [Context menu](#context-menu)
12. [Clipboard](#clipboard)
13. [Debugging and logging](#debugging-and-logging)
14. [Documentation](#documentation)
15. [Dragging](#dragging)
16. [Editor components](#editor-components)
17. [Editor utils](#editor-utils)
18. [Firebase](#firebase)
19. [File upload](#file-upload)
20. [Form components](#form-components)
21. [Form utils](#form-utils)
22. [Form validation](#form-validation)
23. [Generators](#generators)
24. [Hotkeys](#hotkeys)
25. [Images](#images)
26. [Loaders](#loaders)
27. [Maps](#maps)
28. [Masks](#masks)
29. [Media](#media)
30. [Perfomance](#perfomance)
31. [Pipes](#pipes)
32. [Ratings](#ratings)
33. [Notifications](#notifications)
34. [Pdf](#pdf)
35. [Social Medias](#social-medias)
36. [Storage](#storage)
37. [Scroll](#scroll)
38. [Seo](#seo)
39. [State manager](#state-manager)
40. [Tree](#tree)
41. [Table components](#table-components)
42. [Table utils](#table-utils)
43. [Translates](#translates)
44. [Ui Libraries](#ui-libraries)
45. [Ui components](#ui-components)
46. [Ui layouts](#ui-layouts)
47. [Utils](#utils)

### Advertising
1. [ng2-adsense](https://www.npmjs.com/package/ng2-adsense)

### Animation
1. [ng-animate](https://www.npmjs.com/package/ng-animate)
2. [ngx-teximate](https://www.npmjs.com/package/ngx-teximate)
3. [angular-animations](https://www.npmjs.com/package/angular-animations)

### App tour
1. [angular-shepherd](https://www.npmjs.com/package/angular-shepherd)
2. [ngx-app-tour](https://github.com/hamdiwanis/ngx-app-tour)

### Auth
1. [@ngx-auth/core](https://www.npmjs.com/package/@ngx-auth/core)
2. [ngx-permissions](https://www.npmjs.com/package/ngx-permissions)
3. [@auth0/angular-jwt](https://www.npmjs.com/package/@auth0/angular-jwt)
4. [@casl/angular](https://www.npmjs.com/package/@casl/angular)

### Avatar
1. [ngx-avatar](https://www.npmjs.com/package/ngx-avatar)

### Backend
1. [ngx-restangular](https://www.npmjs.com/package/ngx-restangular)
2. [angular2-query-builder](https://www.npmjs.com/package/angular2-query-builder)

### Calendars
1. [@fullcalendar/angular](https://www.npmjs.com/package/@fullcalendar/angular)
2. [angular-calendar](https://www.npmjs.com/package/angular-calendar)

### Carousel
1. [ngx-flicking](https://www.npmjs.com/package/@egjs/ngx-flicking)
2. [@ngu/carousel](https://www.npmjs.com/package/@ngu/carousel)
3. [ngx-swiper-wrapper](https://www.npmjs.com/package/ngx-swiper-wrapper)

### Charts
1. [ng2-charts](https://www.npmjs.com/package/ng2-charts)
2. [@swimlane/ngx-charts](https://www.npmjs.com/package/@swimlane/ngx-charts)

### Config
1. [@ngx-config/core](https://www.npmjs.com/package/@ngx-config/core)

### Context menu
1. [ngx-contextmenu](https://www.npmjs.com/package/ngx-contextmenu)
2. [@ctrl/ngx-rightclick](https://www.npmjs.com/package/@ctrl/ngx-rightclick)

### Clipboard
1. [ngx-clipboard](https://www.npmjs.com/package/ngx-clipboard)

### Debugging and logging
1. [angular-rollbar](https://www.npmjs.com/package/angular-rollbar)
2. [ngx-logger](https://www.npmjs.com/package/ngx-logger)

### Documentation
1. [@compodoc/compodoc](https://www.npmjs.com/package/@compodoc/compodoc)

### Dragging
1. [ng2-dragula](https://www.npmjs.com/package/ng2-dragula)
2. [ngx-drag-to-select](https://www.npmjs.com/package/ngx-drag-to-select)

### Editor components
1. [angular-froala-wysiwyg](https://www.npmjs.com/package/angular-froala-wysiwyg)
2. [@ctrl/ngx-codemirror](https://www.npmjs.com/package/@ctrl/ngx-codemirror)
3. [@kolkov/angular-editor](https://www.npmjs.com/package/@kolkov/angular-editor)

### Editor utils
1. [ngx-highlightjs](https://www.npmjs.com/package/ngx-highlightjs)

### Firebase
1. [@angular/fire](https://www.npmjs.com/package/@angular/fire)
2. [ngx-auth-firebaseui](https://www.npmjs.com/package/ngx-auth-firebaseui)

### File upload
1. [ng2-file-upload](https://www.npmjs.com/package/ng2-file-upload)
2. [ngx-awesome-uploader](https://www.npmjs.com/package/ngx-awesome-uploader)
3. [ngx-dropzone](https://www.npmjs.com/package/ngx-dropzone)
4. [@flowjs/ngx-flow](https://www.npmjs.com/package/@flowjs/ngx-flow)

### Form components
1. [@ctrl/ngx-emoji-mart](https://www.npmjs.com/package/@ctrl/ngx-emoji-mart)
3. [angular2-multiselect-dropdown](https://www.npmjs.com/package/angular2-multiselect-dropdown)
4. [@ng-select/ng-select](https://www.npmjs.com/package/@ng-select/ng-select)
5. [ng2-select](https://www.npmjs.com/package/ng2-select)
6. [ngx-color](https://www.npmjs.com/package/ngx-color)
7. [ngx-color-picker](https://www.npmjs.com/package/ngx-color-picker)
8. [angular2-multiselect-dropdown](https://www.npmjs.com/package/angular2-multiselect-dropdown)
9. [@ngneat/edit-in-place](https://www.npmjs.com/package/@ngneat/edit-in-place)
10. [@ngx-formly/core](https://www.npmjs.com/package/@ngx-formly/core)
15. [ngx-material-timepicker](https://www.npmjs.com/package/ngx-material-timepicker)

### Form utils
1. [ngx-typesafe-forms](https://www.npmjs.com/package/ngx-typesafe-forms)
2. [ngx-image-cropper](https://www.npmjs.com/package/ngx-image-cropper)
3. [@rxweb/types](https://www.npmjs.com/package/@rxweb/types)
4. [@ngneat/forms-manager](https://www.npmjs.com/package/@ngneat/forms-manager)
5. [ngx-moment](https://www.npmjs.com/package/ngx-moment)

### Form validation
1. [ngx-custom-validators](https://www.npmjs.com/package/ngx-custom-validators)
2. [ngx-validators](https://www.npmjs.com/package/ngx-validators)
3. [ngx-phone-validators](https://www.npmjs.com/package/ngx-phone-validators)

### Generators
1. [unique-names-generator](https://www.npmjs.com/package/unique-names-generator)
2. [chance](https://www.npmjs.com/package/chance)
3. [uuid](https://www.npmjs.com/package/uuid)
4. [@techiediaries/ngx-qrcode](https://www.npmjs.com/package/@techiediaries/ngx-qrcode)

### Hotkeys
1. [@ngneat/hotkeys](https://www.npmjs.com/package/@ngneat/hotkeys)

### Images
1. [ngx-masonry](https://www.npmjs.com/package/ngx-masonry)
2. [ngx-gallery](https://www.npmjs.com/package/ngx-gallery)
3. [ng-lazyload-image](https://www.npmjs.com/package/ng-lazyload-image)
4. [ngx-img-fallback](https://www.npmjs.com/package/ngx-img-fallback)
5. [ng2-safe-img](https://www.npmjs.com/package/ng2-safe-img)
6. [@ngneat/svg-icon](https://www.npmjs.com/package/@ngneat/svg-icon)

### Loaders
1. [angular-epic-spinners](https://www.npmjs.com/package/angular-epic-spinners)
2. [angular2-busy](https://www.npmjs.com/package/angular2-busy)
3. [angular2-promise-buttons](https://www.npmjs.com/package/angular2-promise-buttons)
5. [ngx-progressbar](https://www.npmjs.com/package/ngx-progressbar)
6. [ngx-skeleton-loader](https://www.npmjs.com/package/ngx-skeleton-loader)
7. [ngx-perfect-scrollbar](https://www.npmjs.com/package/ngx-perfect-scrollbar)
8. [ng2-slim-loading-bar](https://www.npmjs.com/package/ng2-slim-loading-bar)

### Maps
1. [angular-cesium](https://www.npmjs.com/package/angular-cesium)
2. [@agm/core](https://www.npmjs.com/package/@agm/core)
3. [ngx-mapbox-gl](https://www.npmjs.com/package/ngx-mapbox-gl)
4. [@angular/google-maps](https://www.npmjs.com/package/@angular/google-maps)

### Masks
1. [ngx-mask](https://www.npmjs.com/package/ngx-mask)
2. [angular-imask](https://www.npmjs.com/package/angular-imask)

### Media
1. [ngx-youtube-player](https://www.npmjs.com/package/ngx-youtube-player)
2. [ngx-audio-player](https://www.npmjs.com/package/ngx-audio-player)

### Perfomance
1. [angular-performance-checklist](https://github.com/mgechev/angular-performance-checklist)
2. [angular-idle-preload](https://www.npmjs.com/package/angular-idle-preload)

### Pipes
1. [ngx-filter-pipe](https://www.npmjs.com/package/ngx-filter-pipe)
2. [ngx-pipes](https://www.npmjs.com/package/ngx-pipes)
3. [angular-pipes](https://www.npmjs.com/package/angular-pipes)
4. [ngx-linky](https://www.npmjs.com/package/ngx-linky)
5. [ngx-order-pipe](https://www.npmjs.com/package/ngx-order-pipe)
6. [ngx-linkifyjs](https://www.npmjs.com/package/ngx-linkifyjs)

### Ratings
1. [ngx-bar-rating](https://github.com/MurhafSousli/ngx-bar-rating)

### Notifications
1. [ng-snotify](https://www.npmjs.com/package/ng-snotify)
2. [ngx-popper](https://www.npmjs.com/package/ngx-popper)
3. [ngx-toastr](https://www.npmjs.com/package/ngx-toastr)

### Pdf
1. [ng2-pdf-viewer](https://www.npmjs.com/package/ng2-pdf-viewer)

### Social Medias
1. [ngx-sharebuttons](https://www.npmjs.com/package/ngx-sharebuttons)

### Storage
1. [angular-2-local-storage](https://www.npmjs.com/package/angular-2-local-storage)
2. [ngx-webstorage](https://www.npmjs.com/package/ngx-webstorage)
3. [ngx-store](https://www.npmjs.com/package/ngx-store)

### Scroll
1. [ngx-infinite-scroll](https://www.npmjs.com/package/ngx-infinite-scroll)
2. [angular2-infinite-scroll](https://www.npmjs.com/package/angular2-infinite-scroll)

### Seo
1. [@ngx-meta/core](https://www.npmjs.com/package/@ngx-meta/core)
2. [xng-breadcrumb](https://www.npmjs.com/package/xng-breadcrumb)
3. [@scullyio](https://www.npmjs.com/package/@scullyio/init)
4. [@ngx-cache](https://www.npmjs.com/package/@ngx-cache/core)
5. [angulartics2](https://www.npmjs.com/package/angulartics2)

### State manager
1. [@datorama/akita](https://www.npmjs.com/package/@datorama/akita)
2. [@ngxs/store](https://www.npmjs.com/package/@ngxs/store)
3. [@ngrx/store](https://www.npmjs.com/package/@ngrx/store)

### Tree
1. [ngx-treeview](https://www.npmjs.com/package/ngx-treeview)
2. [@circlon/angular-tree-component](https://www.npmjs.com/package/@circlon/angular-tree-component)

### Table components
1. [ag-grid-community](https://www.npmjs.com/package/ag-grid-community)
2. [ng2-handsontable](https://www.npmjs.com/package/ng2-handsontable)
3. [ng2-smart-table](https://www.npmjs.com/package/ng2-smart-table)
4. [ng2-table](https://www.npmjs.com/package/ng2-table)
5. [@swimlane/ngx-datatable](https://www.npmjs.com/package/@swimlane/ngx-datatable)

### Table utils
1. [ng-table-virtual-scroll](https://www.npmjs.com/package/ng-table-virtual-scroll)
2. [ngx-pagination](https://www.npmjs.com/package/ngx-pagination)

### Translates
1. [@angular/localize](https://www.npmjs.com/package/@angular/localize)
2. [@ngneat/transloco](https://www.npmjs.com/package/@ngneat/transloco)

### Ui Libraries
1. [ej2](https://ej2.syncfusion.com/home/angular.html)
2. [nebular](https://akveo.github.io/nebular/docs/getting-started/what-is-nebular#what-is-nebular)
3. [primeng](https://primefaces.org/primeng/showcase/#/setup)
4. [angular material extensions](https://github.com/angular-material-extensions)
5. [jgwidgets](https://www.jqwidgets.com/angular/)
6. [terada](https://teradata.github.io/covalent/v3/#/)
7. [mdbootstrap](https://mdbootstrap.com/docs/angular/)
8. [ng-bootstrap](https://ng-bootstrap.github.io/#/home)

### Ui components
1. [ngx-countdown](https://www.npmjs.com/package/ngx-countdown)
2. [angular-archwizard](https://www.npmjs.com/package/angular-archwizard)
3. [ngx-scrolltop](https://www.npmjs.com/package/ngx-scrolltop)

### Ui layouts
1. [angular-fullpage](https://www.npmjs.com/package/@fullpage/angular-fullpage)
2. [angular-split](https://www.npmjs.com/package/angular-split)

### Utils
1. [until-destroy](https://github.com/ngneat/until-destroy)
2. [lodash](https://www.npmjs.com/package/lodash)

## Related Projects:
1. [nx-starter](https://github.com/happ-agency/nx-starter)
2. [nx-web-starter](https://github.com/happ-agency/nx-web-starter)
3. [nx-nestjs-starter](https://github.com/happ-agency/nx-nestjs-starter)
4. [nx-angular-nestjs-starter](https://github.com/happ-agency/nx-angular-nestjs-starter)

## Additional Info

> You can check additional info in [nx-starter](https://github.com/happ-agency/nx-starter)
