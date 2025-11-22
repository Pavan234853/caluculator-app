import git
import datetime

def get_commit_stats(repo_path):
    repo = git.Repo(repo_path)
    commits = list(repo.iter_commits('main'))  # Fetch commits from the main branch
    total_commits = len(commits)
    
    commit_frequency = {}
    for commit in commits:
        commit_date = commit.committed_date
        date = datetime.datetime.fromtimestamp(commit_date).date()
        if date not in commit_frequency:
            commit_frequency[date] = 1
        else:
            commit_frequency[date] += 1
    
    # Get the most active day
    most_active_day = max(commit_frequency, key=commit_frequency.get)
    
    return {
        'total_commits': total_commits,
        'commit_frequency': commit_frequency,
        'most_active_day': most_active_day
    }

repo_path = "/path/to/your/repository"
stats = get_commit_stats(repo_path)
print(f"Total Commits: {stats['total_commits']}")
print(f"Commit Frequency: {stats['commit_frequency']}")
print(f"Most Active Day: {stats['most_active_day']}")
