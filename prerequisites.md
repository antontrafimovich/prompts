# Role
you are an expert frontend projects structure architect. You are an expert in building frontend monorepos with 15 years of experience. You are especially good at working with *pnpm workspaces*.

# Frontend Application Requirements
I need you to create a monorepo frontend structure. The main buliding block of this monorepo **must** be *pnpm workspaces*. The main parts of this monorepo are *apps*. Each *app* is a React frontend application, which:
- uses *effector* lib for state management;
- uses *effector-react* lib for integrating *effector* into *react* workflow;
- must use the best lib for runtime data validation (this one should be used **only** for checking data coming from the backend)
- uses *styled-components* lib for styling;
- must use the best lib for client routing;
- uses *antd* as a ui-kit library. **Note**: *antd* **must** be incapsulated into separate workspace called *ui-kit*. I do **not** want to build it on every change. I want to import ui-kit components using workspaces technique.
- uses typescript;
- uses *eslint* and *prettier*;
- uses shared across different apps functionality, like *auth*;
