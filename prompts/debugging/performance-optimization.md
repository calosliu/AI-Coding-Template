# Performance Optimization Prompt

Use this template to get help optimizing slow code or system performance issues.

## Prompt Template

```
I need help optimizing [CODE/SYSTEM] that has performance issues.

## Performance Problem
**Issue**: [What is slow or inefficient]
**Current Performance**: [Metrics: response time, throughput, resource usage]
**Target Performance**: [Desired metrics]
**Impact**: [User experience, cost, etc.]

## Environment
- **Language/Framework**: [Version]
- **Hardware**: [CPU, RAM, relevant specs]
- **Scale**: [Data volume, concurrent users, etc.]

## Code to Optimize
```[language]
[Paste code with performance issues]
```

## Performance Data
**Profiling Results**: [If available]
```
[Paste profiler output, timing data, or metrics]
```

**Bottleneck**: [Where is the slowdown - database, computation, I/O, etc.]

## Constraints
- **Backward Compatibility**: [Must maintain / Can break]
- **Resource Limits**: [Memory, CPU, budget constraints]
- **Code Changes**: [Can refactor / Minor changes only]
- **Dependencies**: [Can add libraries / Must use existing only]

## Context
- **Typical Input**: [Size, characteristics of typical data]
- **Edge Cases**: [Largest/smallest inputs to handle]
- **Frequency**: [How often this code runs]

## Optimization Goals
Prioritize:
1. [e.g., Reduce latency]
2. [e.g., Reduce memory usage]
3. [e.g., Increase throughput]

## Investigation Requests
Please:
1. Analyze the performance bottleneck
2. Explain why it's slow
3. Suggest optimization strategies
4. Provide optimized code
5. Estimate performance improvement
6. Suggest monitoring/profiling approaches
```

## Example Usage

```
I need help optimizing a data processing function that has severe performance issues.

## Performance Problem
**Issue**: Function that processes and aggregates user activity logs is extremely slow
**Current Performance**: 
- Processing 1M records takes ~45 minutes
- Memory usage peaks at 8GB
- CPU usage: 25% (single threaded)
**Target Performance**: 
- Process 1M records in < 5 minutes
- Memory usage < 2GB
- Better CPU utilization
**Impact**: 
- Blocking daily report generation
- Reports delivered late to customers
- High cloud compute costs

## Environment
- **Language/Framework**: Python 3.11 with Pandas 2.0
- **Hardware**: AWS EC2 t3.2xlarge (8 vCPU, 32GB RAM)
- **Scale**: 
  - 1-5M records per day
  - Each record ~200 bytes
  - Need to aggregate by user and by day

## Code to Optimize
```python
import pandas as pd
from datetime import datetime

def process_activity_logs(log_file_path):
    """Process user activity logs and generate aggregated metrics"""
    
    # Read all logs into memory
    df = pd.read_csv(log_file_path)
    
    results = []
    
    # Process each user individually
    for user_id in df['user_id'].unique():
        user_data = df[df['user_id'] == user_id]
        
        # Process each day for this user
        for date in user_data['date'].unique():
            daily_data = user_data[user_data['date'] == date]
            
            # Calculate metrics
            total_events = len(daily_data)
            total_duration = daily_data['duration'].sum()
            unique_pages = daily_data['page_url'].nunique()
            
            # Calculate engagement score (complex calculation)
            engagement_score = 0
            for _, row in daily_data.iterrows():
                score = row['duration'] * 0.1
                if row['event_type'] == 'click':
                    score *= 1.5
                elif row['event_type'] == 'purchase':
                    score *= 3.0
                engagement_score += score
            
            results.append({
                'user_id': user_id,
                'date': date,
                'total_events': total_events,
                'total_duration': total_duration,
                'unique_pages': unique_pages,
                'engagement_score': engagement_score
            })
    
    # Convert to DataFrame and save
    results_df = pd.DataFrame(results)
    results_df.to_csv('processed_results.csv', index=False)
    
    return results_df

# Called daily with previous day's logs
process_activity_logs('/data/logs/activity_2024-01-15.csv')
```

## Performance Data
**Profiling Results**:
```
Line #      Hits         Time  Per Hit   % Time  Line Contents
==============================================================
10             1    2000000.0 2000000.0     4.4  df = pd.read_csv(log_file_path)
15        100000   5000000.0      50.0    11.1  user_data = df[df['user_id'] == user_id]
18       1000000  15000000.0      15.0    33.3  daily_data = user_data[user_data['date'] == date]
28       1000000  20000000.0      20.0    44.4  for _, row in daily_data.iterrows()
42             1   3000000.0 3000000.0     6.7  results_df.to_csv(...)
```

**Bottleneck**: 
- Nested loops with repeated DataFrame filtering (lines 15, 18)
- Row-by-row iteration with iterrows() (line 28)
- Creating many small filtered DataFrames

## Constraints
- **Backward Compatibility**: Can change internals but must produce same output format
- **Resource Limits**: AWS instance cost needs to decrease (prefer better algorithm over bigger machine)
- **Code Changes**: Can completely refactor if needed
- **Dependencies**: Can add libraries (Dask, Polars, etc.) if significant improvement

## Context
- **Typical Input**: 
  - 1-5M rows per file
  - ~50K unique users
  - CSV with columns: user_id, date, event_type, page_url, duration
- **Edge Cases**: 
  - Some users have 10K+ events per day
  - Some days have 10M+ events total
- **Frequency**: Runs once per day (overnight batch job)

## Optimization Goals
Prioritize:
1. Reduce execution time (most important - blocking customers)
2. Reduce memory usage (secondary - keep under 4GB to use smaller instance)
3. Maintain accuracy (cannot compromise on correctness)

## Investigation Requests
Please:
1. Analyze the performance bottleneck in detail
2. Explain why the nested filtering and iterrows() are so slow
3. Suggest optimization strategies (vectorization, better pandas usage, alternative libraries)
4. Provide optimized code with comments explaining improvements
5. Estimate performance improvement for each optimization
6. Suggest monitoring/profiling approaches for future optimization
```

## Tips

- **Measure First**: Profile before optimizing to find real bottlenecks
- **Quantify**: Provide actual metrics, not just "slow"
- **Context**: Explain scale and constraints
- **Trade-offs**: Be clear about what you can and can't change
- **Realistic Goals**: Set achievable performance targets
