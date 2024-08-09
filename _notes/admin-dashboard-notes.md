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
    <DropdownMenuItem>
      <Link href='/profile'>Profile</Link>
    </DropdownMenuItem>
    <DropdownMenuItem>
      <Link href='/auth'>Logout</Link>
    </DropdownMenuItem>
  </DropdownMenuContent>
</DropdownMenu>
```

# 5. Sidebar
- Added `components/Sidebar.tsx`
- imported sidebar into main `layout.tsx`
- rendering:
  - it's displayed in a wrapping div with display flex
  - some tailwind classes to hide it on small screens
```JSX 
<div className="flex">
  <div className='hidden md:block h-[100vh'>
    <Sidebar />
  </div>
  <div className="p-5 w-full md:max-w-[1140px]">
    {children}
  </div>
</div>
```
## Icons using Lucide-React Package
to install:
```shell client
npm i lucide-react
```
## Command Component
- to install
```shell client
npx shadcn-ui@latest add command
```
- imports
```JSX components/Sidebar.tsx
import {
  Command,
  CommandDialog,
  CommandEmpty,
  CommandGroup,
  CommandInput,
  CommandItem,
  CommandList,
  CommandSeparator,
  CommandShortcut,
} from "@/components/ui/command";
import { LayoutDashboard, Newspaper, Folders, CreditCard, Settings, User } from "lucide-react";
import Link from "next/link";
```

- rendering:
``` JSX components/Sidebar.tsx
<Command className='bg-secondary rounded-none'>
  <CommandInput placeholder="Type a command or search..." />
  <CommandList>
    <CommandEmpty>No results found.</CommandEmpty>
    <CommandGroup heading="Suggestions">
      <CommandItem>
        <LayoutDashboard className="mr-2 h-4 w-4" />
        <Link href='/'>Dashboard</Link>
      </CommandItem>
      <CommandItem>
        <Newspaper className="mr-2 h-4 w-4" />
        <Link href='/posts'>Posts</Link>
      </CommandItem>
      <CommandItem>
        <Folders className="mr-2 h-4 w-4" />
        <Link href='#'>Categories</Link>
      </CommandItem>
    </CommandGroup>
    <CommandSeparator />
    <CommandGroup heading="Settings">
      <CommandItem>
        <User className='mr-2 h-4 w-4'/>
        <span>Profile</span>
        <CommandShortcut>⌘P</CommandShortcut>
      </CommandItem>
      <CommandItem>
        <CreditCard className='mr-2 h-4 w-4'/>
        <span>Billing</span>
        <CommandShortcut>⌘B</CommandShortcut>
      </CommandItem>
      <CommandItem>
        <Settings className='mr-2 h-4 w-4'/>
        <span>Settings</span>
        <CommandShortcut>⌘S</CommandShortcut>
      </CommandItem>
    </CommandGroup>
  </CommandList>
</Command>
```

# 6. Dashboard Cards
- Added `components/dashboard` folder to house all the dashboard components

## Dashboard Card component
- installed card component from shadcn-ui
```shell client
npx shadcn-ui@latest add card
```
- added `components/dashboard/DashboardCard.tsx`
  - uses `Card` and `CardContent` from the card component
  - uses an newspaper icon
``` JSX components/dashboard/DashboardCard.tsx
import { Card, CardContent } from '@/components/ui/card';
import { Newspaper } from 'lucide-react';

const DashboardCard = () => {
  return ( 
    <Card className="bg-slate-100 dark:bg-slate-800 p-4 pb-0">
      <CardContent>
        <h3 className="text-3xl text-center mb-4 font-bold text-slate-500 dark:text-slate-200">Posts</h3>
        <div className="flex gap-5 justify-center items-center">
          <Newspaper className="text-slate-500" size={72}/>
          <h3 className="text-5xl font-semibold text-slate-500 dark:text-slate-200">200</h3>
        </div>
      </CardContent>
    </Card> 
  );
}
 
export default DashboardCard;
```

- imported into homepage
``` JSX page.tsx
<div className="flex flex-col md:flex-row justify-between gap-5 mb-5">
  <DashboardCard></DashboardCard>
</div>
``` 

## DashboardCard props for reusability of component
- added an interface `DashboardCardProps`
  - for icon we import `LucideIcon` from lucide-react.
    - this allows us to specify it as ReactElement in the interface | `icon: React.ReactElement<LucideIcon>`
``` TSX DashboardCard.tsx
import { LucideIcon } from 'lucide-react';

interface DashboardCardProps {
  title: string;
  count: number;
  icon: React.ReactElement<LucideIcon>
}
```
- Rendered the props
``` TSX DashboardCard.tsx
const DashboardCard = ({ title, count, icon }: DashboardCardProps) => {
  return ( 
    <Card className="bg-slate-100 dark:bg-slate-800 p-4 pb-0">
      <CardContent>
        <h3 className="text-3xl text-center mb-4 font-bold text-slate-500 dark:text-slate-200">{title}</h3>
        <div className="flex gap-5 justify-center items-center">
          {icon}
          <h3 className="text-5xl font-semibold text-slate-500 dark:text-slate-200">{count}</h3>
        </div>
      </CardContent>
    </Card> 
  );
}
```
- modified render of DashboardCard in the HomePage to specify props
```tsx page.tsx
<DashboardCard 
  title='Posts' count={100} 
  icon={<Newspaper className="text-slate-500" size={72}/>}>
</DashboardCard>
```





