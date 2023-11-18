# Chirp

This project is an emoji-only Twitter clone built with the T3 stack, tRPC, Clerk, Planetscale, Axiom, and Upstash. 
Implementation is based on Theo's guide: https://www.youtube.com/watch?v=YkOSUVzOAA4. 

It is deployed to https://t3-tutorial-rouge.vercel.app/

## Things I learned while working on this project: 
- Use debug mode for `authMiddleware`
    - Clerk was blocking access to posts and accessing userList. 
    - I thought this was a config issue, and chased a solution for a few hours.
    - I finally tried adding `debug: true` to the clerk `authMiddleware` in `middleware.ts`. This revealed I had accidentally added a character to the secret key for clerk in the local .env file. 
- Clerk dashboard is where all the user management is done
    - My users all had `username` as `null`
    - Enabled usernames from Clerk dashboard
- `dayjs` is super easy for relative time
- `zod` does input shape validation
    - If using tRPC for api and `zod` for input form validation, likely need to specify shape of error message
    - See `trpc.ts` `errorFormatter` and https://trpc.io/docs/server/error-formatting#adding-custom-formatting for example
- Form submission in React likes to use `useState` for form state
    - see `CreatePostWizard` component for an example
- Linters are kind of great 
- The `disabled` attribute for html elements is powerful
    - See `isPosting` in `index.tsx` for how to prevent duplicated actions during async ops
- I have a bit more to learn about server side processing. 
    - The Next.js SSG helpers process page data and cache it 
    - These can be used to prefetch page data, then deliver it to client before loading
    - This apparently also helps with SEO 
    - I'm still ignorant about the details here, and further reading of docs is needed 
    - See https://nextjs.org/docs/pages/building-your-application/rendering/static-site-generation as a starting point
- Separate linting, deployment, etc. to speed up deployment. 

## Overview of deployment: 
- Host serverless on Vercel
- Planetscale MySQL db with prisma ORM
- Auth with Clerk 
- Axiom for logging
- Upstash for rate limiting (can also be used for redis, cron jobs, etc.) 
