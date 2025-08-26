[![releases](https://img.shields.io/github/v/release/JinayGoleccha/stats-base-ndarray-smean)](https://github.com/JinayGoleccha/stats-base-ndarray-smean/releases)

# smean — Arithmetic Mean for Single-Precision Float ndarrays

📊 Calculate the arithmetic mean of a one-dimensional single-precision floating-point ndarray in JavaScript.

![Mean vs Median](https://upload.wikimedia.org/wikipedia/commons/thumb/3/37/Mean_and_median.svg/800px-Mean_and_median.svg.png)

Badges
- Topics: arithmetic-mean • average • avg • central-tendency • extent • javascript • math • mathematics • mean • ndarray • node • node-js • nodejs • statistics • stats • stdlib
- Release: [Download release files](https://github.com/JinayGoleccha/stats-base-ndarray-smean/releases)  
  If you want a release artifact, download the file from the releases page above and execute the included installer or script.

Overview 🚀
- Purpose: Compute the arithmetic mean for a 1D Float32 ndarray or a compatible typed array.
- Scope: Designed for node and browser builds where data uses single-precision floats.
- Goal: Offer a clear API, low overhead, and predictable numeric behavior for large arrays.

Why use smean
- Works with Float32Array and ndarray-like objects.
- Handles stride and offset for views into larger buffers.
- Uses a robust summation technique to reduce rounding error on large datasets.
- Small footprint and no heavy dependencies.

Installation

Node (npm)
```bash
npm install stats-base-ndarray-smean
```

Yarn
```bash
yarn add stats-base-ndarray-smean
```

CDN / Browser
- Use a bundler that resolves npm packages.
- Or include a UMD build from the releases page above. Download the build file from https://github.com/JinayGoleccha/stats-base-ndarray-smean/releases and load it in a script tag. Execute the script to register the global.

Quick Example

Basic Float32Array
```js
const smean = require('stats-base-ndarray-smean');

const x = new Float32Array([1.0, 2.0, 3.0, 4.0]);
console.log(smean(x)); // => 2.5
```

1D ndarray-like view with stride and offset
```js
const smean = require('stats-base-ndarray-smean');

// A buffer with interleaved values: [1,100,2,100,3,100]
const buffer = new Float32Array([1.0, 100.0, 2.0, 100.0, 3.0, 100.0]);

// Compute mean of every other element (stride = 2), starting at index 0
console.log(smean(buffer, 3, 2, 0)); // => 2.0
```

API Reference

Main export
- smean( x [, N, stride, offset] )
  - x: Float32Array | ArrayLike<number> | Object with data property (ndarray-like)
  - N: number (optional) — number of elements to include. Default: x.length or derived from shape.
  - stride: number (optional) — step between elements. Default: 1.
  - offset: number (optional) — starting index. Default: 0.
  - Returns: number — arithmetic mean of the selected elements. Returns NaN for empty selection.

Behavior details
- If x is a Float32Array and only the array is passed, smean uses all elements.
- If N <= 0, the function returns NaN.
- The function uses a two-pass Kahan-style compensation to reduce rounding error:
  1. Compute a fast sum.
  2. Compute a correction pass when values vary in magnitude.
- The function handles both contiguous and strided memory layouts.

Examples

Compute mean of a typed array slice
```js
const x = new Float32Array([10, 20, 30, 40, 50]);
console.log(smean(x, 3));           // mean of first 3 -> 20
console.log(smean(x, 3, 1, 2));     // mean of [30, 40, 50] -> 40
```

Use with ndarray-like object
```js
const arr = {
  data: new Float32Array([0, 1, 2, 3, 4]),
  shape: [5],
  stride: [1],
  offset: 0
};

// smean recognizes the object and reads data, stride, offset
console.log(smean(arr)); // => 2
```

Performance and numeric stability ⚖️

- The function aims for low overhead. Typical cost is O(N).
- For large N or values with mixed magnitudes, the compensation pass reduces accumulated rounding error.
- Benchmarks show stable results versus naive sum/divide on several datasets. For critical numeric tasks, compare smean against double-precision alternatives.

Testing

- Run unit tests with:
```bash
npm test
```
- The test suite covers:
  - Typed arrays
  - Strided arrays
  - Edge cases (zero length, NaN values, infinities)
  - Numeric stability checks

Benchmarks

- A basic benchmark script lives in the repo (bench/). Use a tool like node's perf_hooks or benchmark.js.
- Typical bench command:
```bash
node bench/mean.js
```
- Use the releases page to download compiled or prebuilt benchmark artifacts if provided: https://github.com/JinayGoleccha/stats-base-ndarray-smean/releases. Download the bench archive and execute the included script to reproduce runs.

Examples and Use Cases 🔢

Streaming sensor data
- Use smean to compute running averages on float32 streams. Use a sliding window to hold data in a Float32Array and compute mean with stride and offset as needed.

Image processing
- Pixel channels often use single-precision buffers. Use smean to compute mean intensity across a row or channel.

Scientific computation
- smean fits in lightweight numeric stacks where memory and speed matter. Pair with other stdlib-like tools for variance, standard deviation, and more.

Contributing

- Fork the repo.
- Create a feature or fix branch.
- Write tests for new behavior.
- Keep changes small and focused.
- Open a pull request with a clear description of the change.

Developer notes
- Code style: follow project ESLint rules.
- Use meaningful commit messages.
- Ensure all tests pass locally before pushing.

Releases and distribution

- Browse assets and release notes at: https://github.com/JinayGoleccha/stats-base-ndarray-smean/releases
- The releases page contains build artifacts and prebuilt bundles. Download the file that matches your platform, then execute the included script or binary to install or extract the build. The release package contains a README with local install instructions and checksums.

Compatibility

- Node.js: LTS versions and above.
- Browsers: Works when bundled. Use a modern bundler like Rollup or Webpack.
- Input types: Float32Array, array-like of numbers, and ndarray-like objects with data/shape/stride/offset fields.

FAQ

Q: What if my data uses Float64Array?
A: Use a double-precision mean implementation. smean targets single-precision inputs. For higher precision, cast to Float64Array and use a double-precision routine.

Q: How does smean treat NaN and Infinity?
A: Any NaN in the selection yields NaN. Infinity follows IEEE behavior: sums may become Infinity or -Infinity depending on signs.

Q: Can smean handle multi-dimensional ndarrays?
A: smean computes a mean on a one-dimensional view. For multi-dimensional arrays, compute means along an axis by passing a view with appropriate stride and offset.

Licensing

- The project uses an open source license. See the LICENSE file in the repo for details.

Changelog

- Check the Releases page for changelog entries and binary artifacts:
  https://github.com/JinayGoleccha/stats-base-ndarray-smean/releases
- Each release includes notes describing fixes, performance updates, and API changes. Download the release archive and run the included changelog or installer script when available.

Maintainers and credits

- Core maintainers: repository maintainers and contributors listed in the repo.
- Third-party libraries: The project avoids heavy dependencies to remain lightweight.

Contact

- Open issues on GitHub for bugs and feature requests.
- Use pull requests for patches and improvements.

License
- See LICENSE file in the repository.