1. **Remove `empty_arrays_mode` field from `kong/plugins/aws-lambda/schema.lua`**:
   - Delete the `empty_arrays_mode` field from the schema.

2. **Remove logic related to `empty_arrays_mode` from `kong/plugins/aws-lambda/handler.lua`**:
   - Remove the backward compatibility check `if conf.empty_arrays_mode == "legacy" then ... end` (around lines 247-258).
   - Remove the `require` for `remove_array_mt_for_empty_table` (around line 21).

3. **Remove `remove_array_mt_for_empty_table` from `kong/plugins/aws-lambda/request-util.lua`**:
   - Delete the `remove_array_mt_for_empty_table` function (around lines 330-352).
   - Remove it from the returned table (line 361).

4. **Update tests in `spec/03-plugins/27-aws-lambda/99-access_spec.lua`**:
   - Remove the `empty_arrays_mode = "legacy"` and `empty_arrays_mode = "correct"` config from plugin route setup.
   - We might need to keep testing that the behavior is indeed "correct" without the backward compatibility layer. Specifically, removing the `legacy` configuration from tests and ensuring arrays are passed as arrays, not empty objects.
   - Ensure the test `invokes a Lambda function with empty array` relies on the default (new) behavior (arrays remain arrays). We will also need to adjust any legacy tests that checked for empty objects instead of arrays.

5. **Pre-commit checks**:
   - Run `pre_commit_instructions` to see testing and review requirements.

6. **Submit**:
   - Commit and push to a branch.
