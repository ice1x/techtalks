In calculus, particularly in the context of integration by parts, `u` and `dv` are parts of the integral that you choose to decompose a more complex integral into simpler parts. The technique is based on the integration by parts formula:

$$
\int u \, dv = uv - \int v \, du
$$

Here’s what each symbol represents:

- `u`: This is a function that you choose to differentiate. It is the part of the integral that will be differentiated.
- `dv`: This is the remaining part of the integral, which you choose to integrate. It is the differential part that will be integrated.

To apply integration by parts:

1. **Identify `u` and `dv`**: Choose which part of the integrand to set as `u` and which part to set as `dv`. This choice often depends on simplifying the resulting integral.
2. **Differentiate `u`**: Compute `du`, the derivative of `u`.
3. **Integrate `dv`**: Compute `v`, the integral of `dv`.
4. **Substitute into the formula**: Apply the integration by parts formula to get the result.

### Example

Suppose you want to evaluate the integral:

$$
\int x e^x \, dx
$$

You can use integration by parts where:

- Let `u = x`, so `du = dx`.
- Let `dv = e^x \, dx`, so `v = e^x`.

Applying the integration by parts formula:

$$
\int x e^x \, dx = x e^x - \int e^x \, dx
$$

Then, integrate `e^x` to get:

$$
\int e^x \, dx = e^x
$$

So:

$$
\int x e^x \, dx = x e^x - e^x + C
$$

Where `C` is the constant of integration.

### Summary

- `u`: The part of the integrand that you differentiate.
- `dv`: The part of the integrand that you integrate.

Choosing `u` and `dv` correctly is key to simplifying and solving the integral.
