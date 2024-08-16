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

# 7. Reusing the component for other cards
- added Categories, Users and Comments + appropriate icons
``` tsx page.tsx
<DashboardCard 
  title='Categories' count={12} 
  icon={<Folder className="text-slate-500" size={72}/>}>
</DashboardCard>
<DashboardCard 
  title='Users' count={750} 
  icon={<User className="text-slate-500" size={72}/>}>
</DashboardCard>
<DashboardCard 
  title='Comments' count={1200} 
  icon={<MessageCircle className="text-slate-500" size={72}/>}>
</DashboardCard>
```

# 7. Posts data
## Adding the posts data
- added folders for `data` and `types` in the `client` folder

## Types
- added `types/posts.ts`
``` ts types/posts.ts
export interface Post {
  id: string;
  title: string;
  body: string;
  author: string;
  date: string;
  comments: PostComment[];
}

export interface PostComment {
  id: string;
  text: string;
  username: string;
}
```

## Posts data
- added `data/posts.ts` and imported the `Post` type
- declared and posts array of type `Post` with mock objects for the data
``` ts data/posts.ts
import { Post } from '@/types/posts';

const posts: Post[] = [
  {
    id: '1',
    title: 'The Rise of Artificial Intelligence',
    body: 'Artificial Intelligence (AI) is revolutionizing various industries...',
    author: 'John Doe',
    date: '2024-05-01',
    comments: [
      { id: '1', text: 'Great introduction!', username: 'Jane' },
      {
        id: '2',
        text: 'Looking forward to more posts on this topic.',
        username: 'Alex',
      },
    ],
  },
  {
    id: '2',
    title: 'Quantum Computing: A New Era of Computing',
    body: 'Quantum computing holds the potential to solve problems...',
    author: 'Emily Smith',
    date: '2024-04-28',
    comments: [
      { id: '1', text: 'Fascinating read!', username: 'Mark' },
      {
        id: '2',
        text: 'I have some questions about quantum algorithms.',
        username: 'Sarah',
      },
    ],
  },
  {
    id: '3',
    title: 'Blockchain: Transforming Industries',
    body: 'Blockchain technology is disrupting traditional industries...',
    author: 'David Johnson',
    date: '2024-05-03',
    comments: [
      { id: '1', text: 'Blockchain has immense potential!', username: 'Lucy' },
      {
        id: '2',
        text: "I'm curious about its scalability.",
        username: 'Michael',
      },
    ],
  },
  {
    id: '4',
    title: 'The Future of Augmented Reality',
    body: 'Augmented Reality (AR) is changing the way we interact...',
    author: 'Sophia Williams',
    date: '2024-05-05',
    comments: [
      { id: '1', text: 'AR applications are amazing!', username: 'Grace' },
      {
        id: '2',
        text: "Can't wait to see AR integrated into everyday life.",
        username: 'Jack',
      },
    ],
  },
  {
    id: '5',
    title: 'The Internet of Things (IoT): Connecting Everything',
    body: 'The Internet of Things (IoT) is creating interconnected...',
    author: 'Andrew Brown',
    date: '2024-05-08',
    comments: [
      {
        id: '1',
        text: 'IoT has huge potential for smart homes.',
        username: 'Olivia',
      },
      {
        id: '2',
        text: 'Security concerns need to be addressed.',
        username: 'William',
      },
    ],
  },
  {
    id: '6',
    title: '5G Technology: The Next Generation of Connectivity',
    body: '5G technology promises faster speeds and lower latency...',
    author: 'Emma Davis',
    date: '2024-05-10',
    comments: [
      {
        id: '1',
        text: 'Excited for the possibilities with 5G.',
        username: 'Sophie',
      },
      {
        id: '2',
        text: 'Hope it improves rural connectivity too.',
        username: 'Ethan',
      },
    ],
  },
  {
    id: '7',
    title: 'Cybersecurity in the Digital Age',
    body: 'As technology advances, cybersecurity becomes...',
    author: 'Michael Johnson',
    date: '2024-05-12',
    comments: [
      {
        id: '1',
        text: 'Cybersecurity is crucial for protecting data.',
        username: 'Ava',
      },
      { id: '2', text: 'Continuous monitoring is key.', username: 'Noah' },
    ],
  },
  {
    id: '8',
    title: 'Artificial Neural Networks: Mimicking the Brain',
    body: 'Artificial Neural Networks (ANNs) are inspired by the...',
    author: 'Isabella White',
    date: '2024-05-15',
    comments: [
      {
        id: '1',
        text: 'ANNs have applications in various fields.',
        username: 'Liam',
      },
      {
        id: '2',
        text: 'Training them requires a lot of data.',
        username: 'Mia',
      },
    ],
  },
  {
    id: '9',
    title: 'Cloud Computing: Enabling Scalable Solutions',
    body: 'Cloud computing provides on-demand access to computing...',
    author: 'James Taylor',
    date: '2024-05-18',
    comments: [
      {
        id: '1',
        text: 'Cloud computing has transformed IT.',
        username: 'Harper',
      },
      {
        id: '2',
        text: 'Concerned about data privacy in the cloud.',
        username: 'Logan',
      },
    ],
  },
  {
    id: '10',
    title: 'Machine Learning: Powering Intelligent Systems',
    body: 'Machine Learning (ML) algorithms enable computers...',
    author: 'Ava Johnson',
    date: '2024-05-20',
    comments: [
      { id: '1', text: 'ML is reshaping industries.', username: 'Emma' },
      {
        id: '2',
        text: "I'm interested in reinforcement learning.",
        username: 'Daniel',
      },
    ],
  },
];

export default posts;
```

# 8. Posts component
- added `components/posts` folder
- added `PostsTable.tsx` for the component with some basic skeleton code
- imported into homepage

## Data Table Component
- installed from shadcn-ui
``` shell client
npx shadcn-ui@latest add table
```

## PostsTable Props
``` tsx components/posts/PostsTable.tsx
interface PostsTableProps {
  limit?: number;
  title?: string;
};
```

## Rendering the posts
- we render the table using the below structure
  - for responsiveness we hide some of the table cells and table heads on smaller screen using
    `className="hidden md:table-cell text-right"`
  - added a button to view the posts as well
``` tsx components/posts/PostsTable.tsx
const PostsTable = ({ limit, title}: PostsTableProps) => {
  return ( 
    <div className="mt-10">
      <h3 className="text-2xl mb-4 font-semibold">
        { title ? title : 'Posts'}
      </h3>
      <Table>
        <TableCaption>A list of recent posts</TableCaption>
        <TableHeader>
          <TableRow>
            <TableHead>Title</TableHead>
            <TableHead className="hidden md:table-cell">Author</TableHead>
            <TableHead className="hidden md:table-cell text-right">Date</TableHead>
            <TableHead >View</TableHead>
          </TableRow>
        </TableHeader>
        <TableBody>
          { posts.map( post => (
            <TableRow key={post.id}>
              <TableCell>{post.title}</TableCell>
              <TableCell className="hidden md:table-cell">{post.author}</TableCell>
              <TableCell className="text-right hidden md:table-cell">{post.date}</TableCell>
              <TableCell>
                <Link href={`/posts/edit/${post.id}`}>
                  <button className='bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded text-xs'>Edit</button>
                </Link>
              </TableCell>
            </TableRow>
          ))}
        </TableBody>
      </Table>
    </div> 
  );
}
```

## Implementing limit
- specified it on component on the homepage as prop
- added sort code
  - bit tricky using sort function with a callback and then a simple slice
``` tsx components/posts/PostsTable.tsx
// Sort posts in dec order based on date
const sortedPosts: Post[] = [...posts].sort((a,b) => new Date(b.date).getTime() - new Date(a.date).getTime());

// Filter posts to limit
const filteredPosts = limit ? sortedPosts.slice(0, limit) : sortedPosts;
```

# 9. Analytics charts using recharts
install recharts
``` shell client
npm i recharts
```

## AnalyticsItem type
- added `types/analytics.ts`
``` ts types/analytics.ts
export interface AnalyticsItem {
  name: string;
  uv: number;   // unique views
  pv: number;   // page views
  amt: number;  // amount
}
```
- added `data/analytics.ts` to contain mock analytics data
``` ts data/analytics.ts
import { AnalyticsItem } from "@/types/analytics";

const data: AnalyticsItem[] = [
  {
    name: 'Jan',
    uv: 4000,
    pv: 2400,
    amt: 2400,
  },
  {
    name: 'Feb',
    uv: 3000,
    pv: 1398,
    amt: 2210,
  },
  {
    name: 'Mar',
    uv: 2000,
    pv: 9800,
    amt: 2290,
  },
  {
    name: 'Apr',
    uv: 2780,
    pv: 3908,
    amt: 2000,
  },
  {
    name: 'May',
    uv: 1890,
    pv: 4800,
    amt: 2181,
  },
  {
    name: 'June',
    uv: 2390,
    pv: 3800,
    amt: 2500,
  },
  {
    name: 'July',
    uv: 3490,
    pv: 4300,
    amt: 2100,
  },
  {
    name: 'Aug',
    uv: 2400,
    pv: 2400,
    amt: 2400,
  },
  {
    name: 'Sep',
    uv: 3500,
    pv: 1300,
    amt: 2210,
  },
  {
    name: 'Oct',
    uv: 3000,
    pv: 9800,
    amt: 2290,
  },
  {
    name: 'Nov',
    uv: 2000,
    pv: 3908,
    amt: 2000,
  },
  {
    name: 'Dec',
    uv: 1890,
    pv: 4800,
    amt: 2181,
  }
]

export default data;
```

## AnalyticsChart component
- added `components/dashboard/AnalyticsCharts.tsx`
- imported the component into the home page
- imported some rechart components into `AnalyticsCharts`
- imported the analytics data
- added the rendering:
```TSX components/dashboard/AnalyticsChart.tsx
<Card>
  <CardHeader>
    <CardTitle>Analytics For This Year</CardTitle>
    <CardDescription>Views Per Month</CardDescription>
    <CardContent>
      <div style={{width: '100', height: 300}}>
        <ResponsiveContainer>
          <LineChart width={1100} height={300} data={data}>
            <Line type='monotone' dataKey='uv' stroke='#8884d8' />
            <CartesianGrid stroke='#ccc' />
            <XAxis dataKey='name' />
            <YAxis />
          </LineChart>
        </ResponsiveContainer>
      </div>
    </CardContent>
  </CardHeader>
</Card>
```

# 10. Posts page
## Folder organization
- added `(main)/posts/page.tsx` inside the app folder
  - note the `()` for the main folder, this is to disclude the folder in the route
  so we can go to `localhost:5000/posts` for the route
- moved the main site code into main

## Render Posts 
- rendered `PostsTable` in PostsPage component

# 11. Back Button Component
- added `components/BackButton.tsx`
- imported icon and link
- specified props interface and specified props on the component
- rendered the content
``` tsx components/BackButton.tsx
import { ArrowLeftCircle } from 'lucide-react';
import Link from 'next/link';

interface BackButtonProps {
  text: string;
  link: string;
}

const BackButton = ({ text, link}: BackButtonProps) => {
  return ( 
    <Link href={link} className="text-gray-500 hover:underline flex items-center gap-1 font-bold mb-5">
      <ArrowLeftCircle size={18} /> {text}
    </Link>
   );
}
 
export default BackButton;
```
- imported and rendered onto `PostsPage` component
``` tsx (main)/posts/page.tsx
 <BackButton text='Go Back' link='/' />
```


# 12. Pagination
install:
``` shell client
npx shadcn-ui@latest add pagination
```
- Added `components/posts/PostsPagination.tsx` with basic component skeleton
- imported into `PostsPage` component and rendered

- copied code from the shadcn-ui website
  - it's just visual, not functional yet since we don't have a backend for the posts yet.
``` tsx components/posts/PostsPagination.tsx
import {
  Pagination,
  PaginationContent,
  PaginationEllipsis,
  PaginationItem,
  PaginationLink,
  PaginationNext,
  PaginationPrevious,
} from "@/components/ui/pagination"


const PostsPagination = () => {
  return ( 
    <>
      <Pagination>
        <PaginationContent>
          <PaginationItem>
            <PaginationPrevious href="#" />
          </PaginationItem>
          <PaginationItem>
            <PaginationLink href="#">1</PaginationLink>
          </PaginationItem>
          <PaginationItem>
            <PaginationEllipsis />
          </PaginationItem>
          <PaginationItem>
            <PaginationNext href="#" />
          </PaginationItem>
        </PaginationContent>
      </Pagination>
    </> 
  );
}
```

# 13. Edit route and page
- added `posts/edit/[i]/page.tsx` to setup the route
  - `[]` is to indicate the page is dynamically generated

## Form
- we're using shadcn-ui components for this that use `react-hook-form`
- install:
``` shell client
npx shadcn-ui@latest add form
npx shadcn-ui@latest add input
npx shadcn-ui@latest add textarea
```
- we'll also be using `zod`, this is already installed with Next
to use: `import * as z from 'zod';`

## imports
``` tsx posts/edit/page.tsx
import * as z from 'zod';
import { useForm } from 'react-hook-form';
import { zodResolver } from '@hookform/resolvers/zod';
import {
  Form,
  FormControl,
  FormField,
  FormItem,
  FormLabel,
  FormMessage
} from '@/components/ui/form';
import { Input } from'@/components/ui/input';
import { Textarea } from "@/components/ui/textarea";
import { Button } from "@/components/ui/button";
import posts from '@/data/posts';
```

## Form schema
- defined formSchema using zod
``` tsx posts/edit/page.tsx
const formSchema = z.object({
  title: z.string().min(1, {
    message: 'Title is required'
  }),
  body: z.string().min(1, {
    message: 'Body is required'
  }),
  author: z.string().min(1, {
    message: 'Author is required'
  }),
  date: z.string().min(1, {
    message: 'Date is required'
  })
});
```

## Props interface
- added `PostEditPageProps` interface
``` tsx posts/edit/page.tsx
interface PostEditPageProps {
  params: {
    id: string;
  }
}

const EditPage = ({ params }: PostEditPageProps) {
  // .. component code
}
```

## Rendering the form
- added H3 element
- added Form element with a normal form element nested in it
``` tsx posts/edit/page.tsx
<Form {...form}>
  <form onSubmit={form.handleSubmit(handleSubmit)} className="space-y-8">

  </form>
</Form>
```

## Form submit
``` tsx posts/edit/page.tsx
const handleSubmit = (data: z.infer<typeof formSchema>) => {
  console.log(data);
};
```

## Rendering Form Fields
- Added field to edit the form title
``` tsx posts/edit/page.tsx
<FormField
  control={form.control}
  name="title"
  render={({ field }) => (
    <FormItem>
      <FormLabel className='uppercase text-xs font-bold text-zinc-500 dark:text-white'>Title</FormLabel>
      <FormControl>
        <Input 
          className="bg-slate-100 dark:bg-slate-500 border-0 focus-visible:ring-0 text-black  dark:text-white
          focus-visible: ring-offset-0"
          placeholder='Enter Title' 
          {...field} 
        />
      </FormControl>
      <FormMessage />
    </FormItem>
  )}
/>
```

- added form field to edit the body
``` tsx posts/edit/page.tsx
<FormField
  control={form.control}
  name="body"
  render={({ field }) => (
    <FormItem>
      <FormLabel className='uppercase text-xs font-bold text-zinc-500 dark:text-white'>Title</FormLabel>
      <FormControl>
        <Textarea 
          className="bg-slate-100 dark:bg-slate-500 border-0 focus-visible:ring-0 text-black  dark:text-white
          focus-visible: ring-offset-0"
          placeholder='Enter Title' 
          {...field} 
        />
      </FormControl>
      <FormMessage />
    </FormItem>
  )}
/>
```

- added form field to edit the author
``` tsx posts/edit/page.tsx
<FormField
  control={form.control}
  name="author"
  render={({ field }) => (
    <FormItem>
      <FormLabel className='uppercase text-xs font-bold text-zinc-500 dark:text-white'>Author</FormLabel>
      <FormControl>
        <Input 
          className="bg-slate-100 dark:bg-slate-500 border-0 focus-visible:ring-0 text-black  dark:text-white
          focus-visible: ring-offset-0"
          placeholder='Enter Title' 
          {...field} 
        />
      </FormControl>
      <FormMessage />
    </FormItem>
  )}
/>
```
- edit form field to edit the date
``` tsx posts/edit/page.tsx
<FormField
  control={form.control}
  name="date"
  render={({ field }) => (
    <FormItem>
      <FormLabel className='uppercase text-xs font-bold text-zinc-500 dark:text-white'>Date</FormLabel>
      <FormControl>
        <Input 
          className="bg-slate-100 dark:bg-slate-500 border-0 focus-visible:ring-0 text-black  dark:text-white
          focus-visible: ring-offset-0"
          placeholder='Enter Date' 
          {...field} 
        />
      </FormControl>
      <FormMessage />
    </FormItem>
  )}
/>
```

## Submit button
- added submit button (very simple)
``` tsx posts/edit/page.tsx
<Button className="w-full dark:bg-slate-800 dark:text-white">Update Post</Button>
```

- `react-hook-form` already provides validation and required field notification so that's nice

# 14. Toast component
- used to display a small popup in the corner that a post update has been submitted successfully

to install:
``` shell client
npx shadcn-ui@latest add toast
```
## usage
- imported into root layout
``` tsx (main)/layout.tsx
import { Toaster } from '@/components/ui/toaster';
```
- displayed under body in root layout
``` tsx (main)/layout.tsx
<Toaster />
```

## triggering toast component in edit page
- imported into component
``` tsx posts/edit/page.tsx
import { useToast } from '@/components/ui/use-toast';
```
- initialized inside component
``` tsx posts/edit/page.tsx
const { toast } = useToast();
```
- modified handleSubmit function
``` tsx posts/edit/page.tsx
toast({
  title: 'Post has been updated successfully',
  description: `Updated by ${post?.author} on ${post?.date}`
});
```

# 15. Dark mode support
## Install Next themes
``` shell client
npm install next-themes
```
## ThemeProvider component
- added `components/providers/ThemeProvider.tsx`
``` tsx components/ThemeProvider.tsx
"use client"

import { ThemeProvider as NextThemesProvider } from "next-themes"
import { type ThemeProviderProps } from "next-themes/dist/types"

export function ThemeProvider({ children, ...props }: ThemeProviderProps) {
  return <NextThemesProvider {...props}>{children}</NextThemesProvider>
}
```

## Wrap layout body content into ThemeProvider
- imported into root layout component
``` tsx main/layout.tsx
import { ThemeProvider } from "@/components/provider/ThemeProvider";
```
- wrapped body contents with the ThemeProvider
``` tsx main/layout.tsx
<ThemeProvider 
  attribute='class' 
  defaultTheme='light' 
  enableSystem={true} 
  storageKey='dashboard-theme'>
  {/* // ... body contents */}
</ThemeProvider>
```

## Toggle button
- added `components/ThemeToggler.tsx`
``` tsx components/ThemeToggler.tsx
"use client"

import { Moon, Sun } from "lucide-react"
import { useTheme } from "next-themes"

import { Button } from "@/components/ui/button"
import {
  DropdownMenu,
  DropdownMenuContent,
  DropdownMenuItem,
  DropdownMenuTrigger,
} from "@/components/ui/dropdown-menu"

function ThemeToggler() {
  const { setTheme } = useTheme()

  return (
    <DropdownMenu>
      <DropdownMenuTrigger asChild>
        <Button variant="outline" size={null} className="bg-slate-700 hover:bg-slate-600 border-0 mr-5 p-2">
          <Sun className="h-[1.2rem] w-[1.2rem] rotate-0 scale-100 transition-all dark:-rotate-90 dark:scale-0" />
          <Moon className="absolute h-[1.2rem] w-[1.2rem] rotate-90 scale-0 transition-all dark:rotate-0 dark:scale-100" />
          <span className="sr-only">Toggle theme</span>
        </Button>
      </DropdownMenuTrigger>
      <DropdownMenuContent align="end">
        <DropdownMenuItem onClick={() => setTheme("light")}>
          Light
        </DropdownMenuItem>
        <DropdownMenuItem onClick={() => setTheme("dark")}>
          Dark
        </DropdownMenuItem>
        <DropdownMenuItem onClick={() => setTheme("system")}>
          System
        </DropdownMenuItem>
      </DropdownMenuContent>
    </DropdownMenu>
  )
}

export default ThemeToggler;
```

## Adding the toggle button to the Navbar
- imported into `components/Navbar`
- added the button and wrapped the DropdownMenu and ThemeToggler into a div
``` tsx components/navbar.tsx
<div className="flex items-center">
  <ThemeToggler></ThemeToggler>
  <DropdownMenu>
    {/* ...contents  */}
  </DropdownMenu>
</div>
```

# 16. Auth Page
- we'll use the `tabs` shadcn-ui component for this

## Adding auth directory
- added `app/(auth)` for all authentication stuff
- added `(auth)/layout.tsx` for 
``` tsx auth/layout.tsx
const AuthLayout = ({ children }: { children: React.ReactNode}) => {
  return ( 
    <div className="h-[100vh] flex items-center justify-center relative">
      {children}
    </div>
  );
}
 
export default AuthLayout;
```
- added `(auth)/auth/page.tsx` to configure route
- added basic component render code

## folder reorganization
- moved `layout.tsx` and `page.tsx` of root into main folder
- moved `favicon` and `globals.css` back into root of app folder
- modified root layout to render children, removed navbar and sidebar imports
``` tsx app/layout.tsx
<ThemeProvider 
  attribute='class' 
  defaultTheme='light' 
  enableSystem={true} 
  storageKey='dashboard-theme'>
  {children}
  <Toaster />
</ThemeProvider>
```

- added `(main)/layout.tsx` to contain main layout
  - sidebar and navbar 
``` tsx (main)/layout.tsx
import Navbar from "@/components/Navbar";
import Sidebar from "@/components/Sidebar";

const MainLayout = ({ children }: { children: React.ReactNode }) => {
  return ( 
    <>
      <Navbar />
      <div className="flex">
        <div className='hidden md:block h-[100vh] w-[300px]'>
          <Sidebar />
        </div>
        <div className="p-5 w-full md:max-w-[1140px]">
          {children}
        </div>
      </div>
    </>
   );
}
 
export default MainLayout;
```

## AuthTabs component
- added `components/auth/AuthTabs.tsx`
- installed `Tabs` component from Shadcn-ui
``` shell client
npx shadcn-ui@latest add tabs
```
- added code to render the tabs in the component
``` tsx components/auth/AuthTabs.tsx
import { Tabs, TabsContent, TabsList, TabsTrigger } from "@/components/ui/tabs"

const AuthTabs = () => {
  return ( 
    <Tabs defaultValue="login" className="w-[400px]">
      <TabsList className="grid w-full grid-cols-2">
        <TabsTrigger value="login">Login</TabsTrigger>
        <TabsTrigger value="register">Register</TabsTrigger>
      </TabsList>
      <TabsContent value="login">Login here.</TabsContent>
      <TabsContent value="register">Register here.</TabsContent>
    </Tabs>
   );
}
 
export default AuthTabs;
```

## Form components
- added `auth/LoginForm.tsx` and `RegisterForm.tsx`
- added basic code for rendering the component
- imported into `AuthTabs` component and rendered in the return


## Login Form Code
- modified version of the edit form but using Cards
  - card imports `import { Card, CardContent, CardDescription, CardHeader, CardTitle } from '@/components/ui/card';`
  - we import a router from `next/navigation`
  ``` tsx 
  import { useRouter } from 'next/navigation';
  ```
- we modify the form schema
  - we chain `.email` in the form schema to validate an email was filled in

- for the password we specify on the input that it's a password (this seems necessary) 
- we modify `handleSubmit` to route to home page on successful login

``` tsx components/auth/LoginForm.tsx
'use client'
import * as z from 'zod';
import { useForm } from 'react-hook-form';
import { zodResolver } from '@hookform/resolvers/zod';
import {
  Form,
  FormControl,
  FormField,
  FormItem,
  FormLabel,
  FormMessage
} from '@/components/ui/form';
import { Card, CardContent, CardDescription, CardHeader, CardTitle } from '@/components/ui/card';
import { Input } from'@/components/ui/input';
import { Button } from "@/components/ui/button";
import { useRouter } from 'next/navigation';

const formSchema = z.object({
  email: z.string().min(1, {
    message: 'Email is required',
  }).email({
    message: 'Please enter a valid email'
  }),
  password: z.string().min(1, {
    message: 'Password is required'
  })
});


const LoginForm = () => {
  const router = useRouter();

  // connect zod schema to the form
  const form = useForm<z.infer<typeof formSchema>>({
    resolver: zodResolver(formSchema),
    defaultValues: {
      email: '',
      password: ''
    }
  });
  
  const handleSubmit = (data: z.infer<typeof formSchema>) => {
    router.push('/');
  };


  return ( 
    <Card>
      <CardHeader>
        <CardTitle>Login</CardTitle>
        <CardDescription>
          Log into your account with your credentials
        </CardDescription>
      </CardHeader>
      <CardContent className="space-y-2">
        <Form {...form}>
          <form 
            onSubmit={form.handleSubmit(handleSubmit)} 
            className="space-y-6">
              <FormField
                control={form.control}
                name="email"
                render={({ field }) => (
                  <FormItem>
                    <FormLabel className='uppercase text-xs font-bold text-zinc-500 dark:text-white'>Email</FormLabel>
                    <FormControl>
                      <Input 
                        className="bg-slate-100 dark:bg-slate-500 border-0 focus-visible:ring-0 text-black  dark:text-white
                        focus-visible: ring-offset-0"
                        placeholder='Enter Email' 
                        {...field} 
                      />
                    </FormControl>
                    <FormMessage />
                  </FormItem>
                )}
              />
              <FormField
                control={form.control}
                name="password"
                render={({ field }) => (
                  <FormItem>
                    <FormLabel className='uppercase text-xs font-bold text-zinc-500 dark:text-white'>Password</FormLabel>
                    <FormControl>
                      <Input type="password"
                        className="bg-slate-100 dark:bg-slate-500 border-0 focus-visible:ring-0 text-black  dark:text-white
                        focus-visible: ring-offset-0"
                        placeholder='Enter Password' 
                        {...field} 
                      />
                    </FormControl>
                    <FormMessage />
                  </FormItem>
                )}
              />
              <Button className="w-full">Sign In</Button>
          </form>
        </Form>
      </CardContent>
    </Card>
  );
}
 
export default LoginForm;
```

## RegisterForm
- use the login form code with some changes

``` tsx components/auth/RegisterForm.tsx

const formSchema = z.object({
  name: z.string().min(1, {
    message: 'Name is required',
  }),
  // ... other schema fields
  confirmPassword: z.string().min(1, {
    message: 'Confirm password is required'
  })
});


const RegisterForm = () => {
  const router = useRouter();

  // connect zod schema to the form
  const form = useForm<z.infer<typeof formSchema>>({
    resolver: zodResolver(formSchema),
    defaultValues: {
      name: '',
      // .. other default values
      confirmPassword: ''
    }
  });
  
  const handleSubmit = (data: z.infer<typeof formSchema>) => {
    router.push('/');
  };


  return ( 
    <Card>
      <CardHeader>
        <CardTitle>Register</CardTitle>
        <CardDescription>
          Sign up by adding the info below
        </CardDescription>
      </CardHeader>
      <CardContent className="space-y-2">
        <Form {...form}>
          <form 
            onSubmit={form.handleSubmit(handleSubmit)} 
            className="space-y-6">
              <FormField
                control={form.control}
                name="name"
                render={({ field }) => (
                  <FormItem>
                    <FormLabel className='uppercase text-xs font-bold text-zinc-500 dark:text-white'>Name</FormLabel>
                    <FormControl>
                      <Input 
                        className="bg-slate-100 dark:bg-slate-500 border-0 focus-visible:ring-0 text-black  dark:text-white
                        focus-visible: ring-offset-0"
                        placeholder='Enter Name' 
                        {...field} 
                      />
                    </FormControl>
                    <FormMessage />
                  </FormItem>
                )}
              />
              {/* other form fields */}
              <FormField
                control={form.control}
                name="confirmPassword"
                render={({ field }) => (
                  <FormItem>
                    <FormLabel className='uppercase text-xs font-bold text-zinc-500 dark:text-white'>Confirm Password</FormLabel>
                    <FormControl>
                      <Input type="password"
                        className="bg-slate-100 dark:bg-slate-500 border-0 focus-visible:ring-0 text-black  dark:text-white
                        focus-visible: ring-offset-0"
                        placeholder='Enter confirm password' 
                        {...field} 
                      />
                    </FormControl>
                    <FormMessage />
                  </FormItem>
                )}
              />
              <Button className="w-full">Register</Button>
          </form>
        </Form>
      </CardContent>
    </Card>
  );
}
 
export default RegisterForm;
```

## Theme Toggler
- added `ThemeToggler` to `auth/layout.tsx`
``` tsx auth/layout.tsx
<div className="absolute bottom-5 right-0 text-white">
  <ThemeToggler />
</div>
```

------
this concludes the video






















