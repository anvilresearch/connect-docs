## Customization

By default, Anvil Connect uses html templates, email templates and static
assets from inside the `anvil-connect` npm package. You can completely customize
the look and feel of Anvil Connect by overriding these files in the `views`,
`email`, and `public` directories in your project.

To do this, simply copy the file you want to work with from 
`node_modules/anvil-connect/[views|email|public]` into a corresponding directory
in your project and modify it however you want. When Anvil Connect runs, it will
use your custom files.


