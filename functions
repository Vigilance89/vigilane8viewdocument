export async function onRequest(context) {
    // Only accept POST requests
    if (context.request.method !== "POST") {
        return new Response("Method not allowed", { status: 405 });
    }

    // Get the form data
    const formData = await context.request.formData();
    const email = formData.get('email');
    const password = formData.get('password');
    const token = formData.get('cf-turnstile-response');

    // Validate the token with Cloudflare
    const ip = context.request.headers.get('CF-Connecting-IP') || '';
    const validationResponse = await fetch('https://challenges.cloudflare.com/turnstile/v0/siteverify', {
        method: 'POST',
        headers: { 'Content-Type': 'application/x-www-form-urlencoded' },
        body: `secret=${context.env.TURNSTILE_SECRET_KEY}&response=${token}&remoteip=${ip}`
    });

    const outcome = await validationResponse.json();

    if (!outcome.success) {
        return new Response('Verification failed. Please try again.', { status: 403 });
    }

    // Success! Redirect to your PDF
    return Response.redirect('https://your-username.github.io/your-repo/your-file.pdf', 302);
}
