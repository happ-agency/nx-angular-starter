# [Nx](https://nx.dev/) Starter

> Core setup for the [nx](https://nx.dev/) applications

1. To install a [pnpm](https://www.npmjs.com/package/pnpm) globally run: `npm install -g pnpm`

	> pnpm - package manager, which is much faster then npm and yarn

2. To install a [nx](https://www.npmjs.com/package/nx) globally run: `pnpm install -g nx`

	> nx - cli for use nx features via terminal
	>
	> use `sudo pnpm install -g nx` if you have access errors

3. To create a [nx workspace](https://nx.dev/) run: `pnpx create-nx-workspace WORKSPACE_NAME --preset=empty --no-nxCloud --package-manager=pnpm --defaultBase=main`

	> 1. `WORKSPACE_NAME` - name of workspace
	> 2. `--preset=empty` - preset to use
	> 3. `--nxCloud` - use nx cloud or not
	> 4. `--package-manager=pnpm` - default package manager
	> 5. `--defaultBase=main` - default git branch

4. Force using [pnpm](https://pnpm.js.org/en/):

	1. Add `.npmrc` in the root folder with:

		```
		engine-strict = true
		```

	2. Add to the `package.json`:

		```
		...
		"license": "...",
		"engines": {
			"yarn": "please-use-pnpm",
			"npm": "please-use-pnpm",
			"pnmp": ">= 15.4.3"
		},
		"scripts": { ... }
		...
		```

	> Force usign `pnpm` throw `please-use-pnpm` error on `npm` or `yarn` use

5. Setup [Git](https://git-scm.com/):

	1. To add origin run: `git remote add origin https://github.com/happ-agency/nx-starter.git`
	2. To set main branch run: `git branch -M main`
	3. To push on git run: `git push -u origin main`
	
6. Setup [GitFlow](https://danielkummer.github.io/git-flow-cheatsheet/index.ru_RU.html):

	1. For production run: `git branch -M main`
	2. For develop run: `git checkout -b develop` (from `main`)
	3. For feature run: `git checkout -b feature/xxx` (from develop)
	4. For fix run: `git checkout -b fix/xxx` (from develop)
	5. For release rus: `git checkout -b release/xxx` (from develop)
	6. For hotfix run: `git checkout -b hotfix/xxx` (from main)
	
	> 1. We work in the `develop` branch.
	> 2. When we start work with the `feature` or `fix`, we create `feature/xxx` or `fix/xxx` from the `develop`.
	> 3. After finish the work we create pull request from the `feat/xxx` or `fix/xxx` to the `develop`.
	> 4. For the relase create `release/xxx` from the `develop`.
	> 5. For the publish create pull request from the `release/xxx` to the `main`.
	> 6. After the publish rebase `main` from the `dev` and rebase `dev` from the `feature/xxx` or `fix/xxx`.
	> 7. `Hotfix` creates from the `main` and create pull request to the `main` after the finish
	> 8. After each changes in the `main` or `develop` we do rebase from connected branches. rebase `main` from `develop`. rebase `develop` from `features` or `fixes`

7. Setup [Husky](https://www.npmjs.com/package/husky):

	1. To install `husky` (only for dev) run: `pnpm i -D husky`
	2. Add to the `package.json`:
		```
		{
			...
			"scripts": { ... },
			"husky": {
				"hooks": {
				 	"pre-commit": "nx lint"
				}
			},
			"private": "...",
			...
		}
		```

8.  Setup [Commitlint](https://yarnpkg.com/package/commitlint):

	1. To install [commitlint](https://yarnpkg.com/package/commitlint) (only for dev) run: `pnpm i -D @commitlint/{config-conventional,cli}`
	2. To install [@happ/commitlint-config]() (only for dev) run: `pnpm i -D @happ/commitlint-config`
	3. Add `.commitlintrc.js` in the root folder with:
	
		```
		module.exports = {
			"extends": ["@happ/commitlint-config"]
		}
		```

	4. Add to the `package.json`:
	
		```
		"husky": {
			"hooks": {
				...
				"pre-commit": "nx lint",
				"commit-msg": "commitlint -E HUSKY_GIT_PARAMS",
				...
			}
		}
		```

	> Commit have to be like `feat: add auth service`, `fix: remove wrong validation`
	>
	> You can find all availale types [here](https://github.com/conventional-changelog/commitlint/tree/master/@commitlint/config-conventional)

9.  Setup [Commitizen](https://yarnpkg.com/package/commitizen) and [cz-format-extension](https://github.com/tyankatsu0105/cz-format-extension):

	1. To install [commitizen](Commitizen) (only for dev) run: `pnpm i -D commitizen cz-format-extension`
	2. To install [@happ/commitizen-config]() config (only for dev) run: `pnpm i -D @happ/commitizen-config`
	3. To make your repo commitizen-friendly run: `commitizen init cz-conventional-changelog --save-dev --save-exact`
	4. Create `.czrc` with:

		```
		{
			"path": "cz-format-extension"
		}
		```

	5. Create `.czferc.js` with:

		```
		module.exports = {
			extends: ['@happ/commitizen-config']
		};
		```

	6. Add to the `package.json`:
	
		```
		{
		...
		"scripts": {
			...
			"git-add": "git add .",
			"git-commit": "cz",
			"git-push": "git push",
			"git": "pnpm run git-add && pnpm run git-commit && pnpm run git-push"
			...
		}
		...
		}
		```

		> Use `cz-format-extension` for customize commitizent questions

10. Setup [Conventional Changelog](https://yarnpkg.com/package/conventional-changelog):

	1. To install [conventional changelog](https://yarnpkg.com/package/conventional-changelog) (only for dev) run: `pnpm i -D conventional-changelog-cli`
	2. To init changelog run: `conventional-changelog -p angular -i CHANGELOG.md -s -r 0`
	3. Add to the `package.json`:

		```
		{
			...
			"scripts": {
				...
				"changelog": "conventional-changelog -p angular -i CHANGELOG.md -s && git add CHANGELOG.md",
				"version": "pnpm run changelog"
				...
			}
			...
		}
		```

    > Use `pnpm version [major|minor|patch]` to change your project's version and generate changelog
    >
    > `version` script runs automatically on `pnpm version major`

11. Setup [Eslint](https://eslint.org/):

	1. To install [eslint](https://eslint.org/) (only for dev) run: `pnpm i -D eslint`
	2. To install [@happ/eslint-config]() (only for dev) run: `pnpm i -D @happ/eslint-config`
	3. To install [@happ/eslint-config-typescript]() (only for dev) run: `pnpm i -D @happ/eslint-config-typescript`
	4. To install [@happ/eslint-config-jest]() (only for dev) run: `pnpm i -D @happ/eslint-jest`
	5. To install [@happ/eslint-config-angular]() (only for dev) run: `pnpm i -D @happ/eslint-config-angular`
	6. To install [@happ/eslint-config-angular-template]() (only for dev) run: `pnpm i -D @happ/eslint-config-angular-template`
	7. To install [@happ/eslint-config-json]() (only for dev) run: `pnpm i -D @happ/eslint-config-json`
	8. To install [@happ/eslint-config-markdown]() (only for dev) run: `pnpm i -D @happ/eslint-config-markdown`
		 > To install it together run: `pnpm i -D @happ/eslint-config @happ/eslint-config-typescript @happ/eslint-config-jest @happ/eslint-config-angular @happ/eslint-config-angular-template @happ/eslint-config-json @happ/eslint-config-markdown`
	9. Create `.eslintrc.js` with:

		```
		module.exports = {
			"overrides": [
				{
					"files": ["*.js", "*.ts"],
					"extends": ["@happ/eslint-config"]
				},
				{
					"files": ["*.ts"],
					"extends": ["@happ/eslint-config-typescript"]
				},
				{
					"files": ["*.spec.js", "*.spec.ts"],
					"extends": ["@happ/eslint-config-jest"]
				},
				{
					"files": ["ANGULAR_DIRECTORY/*.ts"],
					"extends": ["@happ/eslint-config-angular"]
				},
				{
					"files": ["ANGULAR_DIRECTORY/*.html"],
					"extends": ["@happ/eslint-config-angular-template"]
				},
				{
					"files": ["*.json"],
					"extends": ["@happ/eslint-config-json"]
				},
				{
					"files": ["*.md"],
					"extends": ["@happ/eslint-config-markdown"]
				}
			]
		}
		```

12. Setup [Prettier](https://prettier.io/):

	1. To install [prettier](https://prettier.io/) run: `pnpm i -D prettier`
	2. To install [@happ/prettier-config] (only for dev) run: `pnpm i -D @happ/prettier-config`
	3. Create `.prettierrc.js` with:
	
		```
		module.exports = {
			extends: ["@happ/prettier-config"]
		}
		```

13. Setup [Stylelint](https://stylelint.io/):

	1. To install [stylelint](https://stylelint.io/) (only for dev) run: `pnpm i -D stylelint`
	2. To install [@happ/stylelint-config] (only for dev) run: `pnpm i -D @happ/stylelint-config`
	3. Create `.stylelintrc.js` with:
	
		```
		module.exports = {
			extends: ["@happ/stylelint-config"]
		}
		```

14. Setup [EditorConfig](https://editorconfig.org/):

	1. Create `.editorconfig` with:

		```
		root = true
		
		[*]
		end_of_line = lf
		insert_final_newline = true
		indent_style = tab
		indent_size = 2
		```

	> If you are using VS Code you have also install [plugin](https://marketplace.visualstudio.com/items?itemName=EditorConfig.EditorConfig)

### Next Steps

After this steps you can create `apps` and `libs` and register them in:

1. `nx.json` for `nx dependenices`
2. `workspace.json` for `run|buid settings`

Or you can use one of nx plugins for manage it:

1. For [Web]() check [this]()
2. For [Angular]() check [this]()
3. For [NestJS]() check [this]()
4. For [Angular]() + [NestJS]() check [this]()
