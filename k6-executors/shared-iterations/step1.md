_Shared Iterations_ is the most basic of the executors. As can be inferred from the name, the primary focus will be the number of _iterations_ for your test; this is the number of times your test function will be run.

| Option        | Description                                                   | Default |
|---------------|---------------------------------------------------------------|---------|
| `iterations`  | How many times should your test execute?                      | `1`     |
| `maxDuration` | Forcibly stop your test if not finished within this timeframe | `"10m"` |
| `vus`         | Number of virtual users to run concurrently                   | `1`     |

As noted above, the primary objective is to perform your test `iterations` number of times, over a time period not to exceed `maxDuration`. At any time during the test scenario, there should be `vus` iteration(s) happening unless the total desired iterations has been reached. In this scenario, it is possible for some VUs to perform more work than others.

# Creating our script
Let's begin by implementing our `test.js` script.

We'll start with the bare-minimum to use the executor, which only requires that we specify the `executor` itself. From the table above, we see that sensible defaults are provided for the optional settings, so we'll just go with that (for now).

<pre class="file" data-filename="test.js" data-target="replace">// Using the shared-iterations executor
import http from 'k6/http';
import { sleep } from 'k6';

export const options = {
  scenarios: {
    k6_workshop: {
      executor: 'shared-iterations',
    },
  },
};

export default function () {
  http.get('https://test.k6.io/contacts.php');
  sleep(0.5);
}
</pre>

Now that we've defined our basic script, we'll go ahead and run k6:

`k6 run scripts/test.js`{{execute}}

> TODO: Need to figure out how to get a base image that includes k6, so ignore the error for now!

Looking at our results, you should see confirmation that we truly ran a single test iteration.

```bash
  iterations.....................: 1  <snip>
```

# Change the iterations
A test of one request doesn't provide for much, so let's fix that. Let's bump up the number of `iterations` our test will perform.

<pre class="file" data-filename="test.js" data-target="insert" data-marker="executor: 'shared-iterations',">
      executor: 'shared-iterations',
      iterations: 200,
</pre>

Once again, we'll execute the script with k6:

`k6 run scripts/test.js`{{execute}}

Output should confirm that we've run the specified number of iterations!

# Change concurrency
So far, our test has only utilized a single virtual user, or VU. We can update the `vus` option to increase the number of requests being performed simultaneously:

<pre class="file" data-filename="test.js" data-target="insert" data-marker="iterations: 200,">
      iterations: 200,
      vus: 10,
</pre>

Run the script again:

`k6 run scripts/test.js`{{execute}}

The test should now perform the same number of iterations, but in much less time due to the number of concurrent requests.

# Changing the maximum duration
Sometimes scripts can take a while. Whether this is expected or due to a runaway script, time can be money. If a service is not meeting performance requirements (SLAs), there may be no need to keep the test running; it's better to end the test early.

This can be done by managing the `maxDuration` option. By default, we've been getting the maximum duration as 10 minutes. Things should have been completing well within that time. 

Let's enact an aggressive SLA for our test; one that we know from the previous tests that the 200 requests won't finish within: 

<pre class="file" data-filename="test.js" data-target="insert" data-marker="vus: 10,">
      vus: 10,
      maxDuration: '5s',
</pre>

`k6 run scripts/test.js`{{execute}}

With this 5 second requirement, you should see that the test ended before the 200 requests could be completed; there was no need to perform all the requests as we already know the service isn't up to snuff!

# Summary
With this exercise, you should see how to run a very basic test and how you can control the number of iterations, concurrency, and even a bit to show how SLAs can be applied.
