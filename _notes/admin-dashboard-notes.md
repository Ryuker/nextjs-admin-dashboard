# Admin Dashboard Notes
- [video](https://www.youtube.com/watch?v=hhudoSMM0yU&t=570s)
- [shadcn-ui website](https://ui.shadcn.com/)

# 1. Setup
## Creating a new Next app with Tailwind installed
```shell client
npx create-next-app@latest --tailwind
```

# 2. Install Shadcn/UI and initialize
```shell client
npx shadcn-ui@latest init
```

- running the above command adds Shadcn/ui but also a components.json file
  - this is for configuration.

# 3. Bringing in a Shadcn component into the app
```shell
npx shadcn-ui@latest add button
```
- this adds a components folder inside the root folder of the project.
  - note this is not in the app folder.
  - this folder just has the component as a single file, we can customize it

## using the component
- example usage
``` JS
import { Button } from "@/components/ui/button";

<Button 
  variant='destructive' 
  size='lg'
  className='text-blue-400'>Click Me</Button>
```

# 4. Navbar Component
- added `components/Navbar.tsx`
  - added navbar with black background
  - addded logo
``` JS components/Navbar.tsx
import Image from "next/image";
import Link from "next/link";
import logo from '../img/logo.png';

const Navbar = () => {
  return ( 
    <div className="bg-primary dark:bg-slate-700 text-white py-2 px-5 flex justify-between">
      <Link href="/">
        <Image src={logo} alt='Dashboard Logo' width={40} />
      </Link>
    </div>
  );
}
 
export default Navbar;
```

## Avatar component
to install
```shell client
npx shadcn-ui@latest add avatar
```
- added avatar imports to `Navbar`
``` JS
import { Avatar, AvatarImage, AvatarFallback } from '@/components/ui/avatar';
```
- Added avatar rendering into the `Navbar`
  - we use `AvatarImage` to display the image from a src
  - we use `AvatarFallback` to display something when the image can't be loaded for some reason.
``` JS
<Avatar>
  <AvatarImage src='https://github.com/shadcn.png' alt='@shadcn' />
  <AvatarFallback className="text-black">BT</AvatarFallback>
</Avatar>
```

## Dropdown component
- to install
```shell client
npx shadcn-ui@latest add dropdown-menu
```
- imports:
```JSX Navbar.tsx
import {
  DropdownMenu,
  DropdownMenuContent,
  DropdownMenuItem,
  DropdownMenuLabel,
  DropdownMenuSeparator,
  DropdownMenuTrigger,
} from "@/components/ui/dropdown-menu"
```
- rendering: 
  - note we move the Avatar into the `DropdownMenuTrigger`
    - we remove an outline from the trigger with `className="focus:outline-none"`
```JSX Navbar.tsx
<DropdownMenu>
  <DropdownMenuTrigger className="focus:outline-none">
    <Avatar>
    <AvatarImage src='https://github.com/shadcn.png' alt='@shadcn' />
    <AvatarFallback className="text-black">BT</AvatarFallback>
  </Avatar>
  </DropdownMenuTrigger>
  <DropdownMenuContent>
    <DropdownMenuLabel>My Account</DropdownMenuLabel>
    <DropdownMenuSeparator />
    <DropdownMenuItem>Profile</DropdownMenuItem>
    <DropdownMenuItem>Billing</DropdownMenuItem>
    <DropdownMenuItem>Team</DropdownMenuItem>
    <DropdownMenuItem>Subscription</DropdownMenuItem>
  </DropdownMenuContent>
</DropdownMenu>
```




