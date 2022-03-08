# Tailwind CSS

- gives tons of  utility classes
- utility classes are low level, so do single job instead of craeting styled component like in bootstrap
- **text color:** text-{color}-{strength} - text-red-500
- **text size:**  text-{size-info} - text-2xl
- **font weight:** font-{weight-info} - font-bold
- instead of changing default properties, we should add our custom variables in **tailwind.config.js** file
- we can combine all repetitive classes into single class name with ``@apply`` directive
- display property is not used at all, instead directy the values are written. like **flex** or **grid**
- h-screen means viewport height
- hidden keyword is used instead of display none
