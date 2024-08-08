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



