#L3 test on synthetic data

##Test1: baseline linear signal
###Test 2 python code
def generate_synthetic_des_data(n_groups=30,samples_per_group=20,n_features=25,
                                n_informative=5,noise_level=0.1,random_state=42):
    rng = np.random.default_rng(random_state)
    rows = []
    for g in range(n_groups):
        group_shift = rng.normal(0, 1)
        for i in range(samples_per_group):
            X = rng.normal(0, 1, n_features)
            signal = (2.0 * X[0] +-1.5 * X[1] +1.0 * X[2] +0.5 * X[3] +-0.8 * X[4])
            y = signal + group_shift + rng.normal(0, noise_level)
            row = {"group_API": f"API_{g}","y": y}
            for j in range(n_features):
                row[f"f{j}"] = X[j]
            rows.append(row)
    df = pd.DataFrame(rows)
    return df

###Test 1 report
test1_summary.png

##Test2: strong group effect
##Test 2 python code
def generate_synthetic_strong_group(n_groups=30,samples_per_group=20,
                                    n_features=25,noise_level=0.1,random_state=42):
    rng = np.random.default_rng(random_state)
    rows = []
    for g in range(n_groups):
        group_shift = rng.normal(0, 3.0)  
        for _ in range(samples_per_group):
            X = rng.normal(0, 1, n_features)
            signal = (2.0 * X[0]-1.5 * X[1]+1.0 * X[2]+0.5 * X[3]-0.8 * X[4])
            y = signal + group_shift + rng.normal(0, noise_level)
            row = {"group_API": f"API_{g}", "y": y}
            for j in range(n_features):
                row[f"f{j}"] = X[j]
            rows.append(row)
    return pd.DataFrame(rows)

##Test 2 report
test2_summary.png

##Test3: nonlinear signal
###Test 3 python code
def generate_synthetic_nonlinear(n_groups=30,samples_per_group=20,n_features=25,
                                 noise_level=0.1,random_state=42):
    rng = np.random.default_rng(random_state)
    rows = []
    for g in range(n_groups):
        group_shift = rng.normal(0, 1)
        for _ in range(samples_per_group):
            X = rng.normal(0, 1, n_features)
            signal = (
                1.5 * X[0]
                + np.sin(X[1])             # nonlinear
                + 0.5 * (X[2] ** 2)       # nonlinear
                - 1.0 * np.exp(-X[3]**2)  # nonlinear localized
                + 0.7 * X[4]            )
            y = signal + group_shift + rng.normal(0, noise_level)
            row = {"group_API": f"API_{g}", "y": y}
            for j in range(n_features):
                row[f"f{j}"] = X[j]
            rows.append(row)
    return pd.DataFrame(rows)

###Test 3 report
test3_summary.png
 
##Test4: correlated descriptors
###Test 4 python code
def generate_synthetic_correlated(n_groups=30,samples_per_group=20,
                                  n_features=25,noise_level=0.1,random_state=42):
    rng = np.random.default_rng(random_state)
    rows = []
    for g in range(n_groups):
        group_shift = rng.normal(0, 1)
        for _ in range(samples_per_group):
            base = rng.normal(0, 1)
            X = rng.normal(0, 1, n_features)
            X[5] = X[0] + rng.normal(0, 0.05)
            X[6] = X[1] + rng.normal(0, 0.05)
            X[7] = base + rng.normal(0, 0.1)
            X[8] = base + rng.normal(0, 0.1)
            signal = (2.0 * X[0]-1.5 * X[1]+1.0 * X[2]+0.5 * X[3]-0.8 * X[4])
            y = signal + group_shift + rng.normal(0, noise_level)
            row = {"group_API": f"API_{g}", "y": y}
            for j in range(n_features):
                row[f"f{j}"] = X[j]
            rows.append(row)
    return pd.DataFrame(rows)

###Test 4 report
test4_summary.png
 
##Test5: no-signal control
###Test 5 python code
def generate_synthetic_no_signal(n_groups=30,samples_per_group=20,
                                 n_features=25,random_state=42):
    rng = np.random.default_rng(random_state)
    rows = []
    for g in range(n_groups):
        for _ in range(samples_per_group):
            X = rng.normal(0, 1, n_features)
            y = rng.normal(0, 1)  
            row = {"group_API": f"API_{g}", "y": y}
            for j in range(n_features):
                row[f"f{j}"] = X[j]
            rows.append(row)
    return pd.DataFrame(rows)

###Test 5 report
test5_summary.png
 

##Test6: interaction effects
###Test 6 python code
def generate_synthetic_interactions(n_groups=30,samples_per_group=20,
                                    n_features=25,noise_level=0.1,random_state=42):
    rng = np.random.default_rng(random_state)
    rows = []
    for g in range(n_groups):
        group_shift = rng.normal(0, 1)
        for _ in range(samples_per_group):
            X = rng.normal(0, 1, n_features)
            signal = (1.5 * X[0]+ 1.2 * X[1]+ 2.0 * X[0] * X[1]   - 0.8 * X[2] * X[3]+ 0.5 * X[4])
            y = signal + group_shift + rng.normal(0, noise_level)
            row = {"group_API": f"API_{g}", "y": y}
            for j in range(n_features):
                row[f"f{j}"] = X[j]
            rows.append(row)
    return pd.DataFrame(rows)

###Test 6 report
test6_summary.png
