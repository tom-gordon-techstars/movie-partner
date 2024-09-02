<script lang="ts">
    // register/+page.svelte
    import { auth } from '$lib/firebase.client';
    import { createUserWithEmailAndPassword } from 'firebase/auth';
    import { goto } from '$app/navigation';
    import { session } from '$lib/session';
   
    let email: string = '';
    let password: string = '';
   
    async function handleRegister() {
      try {
        const result = await createUserWithEmailAndPassword(auth, email, password);
         const { user } = result;
         session.update((cur: any) => {
          return {
           ...cur,
           user,
           loggedIn: true,
           loading: false
          };
         });
         goto('/');
      } catch (error) {
        console.error('Error during registration:', (error as any).message, (error as any).code);
      }
    }
   </script>
   

   <div class="register-form">
    <form on:submit={handleRegister}>
     <h2>Register</h2>
     <input bind:value={email} type="text" placeholder="Email" />
     <input bind:value={password} type="password" placeholder="Password" />
     <button type="submit">Register</button>
    </form>
   </div>